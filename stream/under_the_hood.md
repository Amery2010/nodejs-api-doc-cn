# 流的内部细节

* [缓冲](#buffering)
* [与 Node.js 早期版本的兼容性](#compatibility_with_older_Nodejs_versions)
* [对象模式](#object_mode)
* [stream.read(0)](#read)
* [stream.push('')](#push)

--------------------------------------------------


<div id="buffering" class="anchor"></div>
## 缓冲

无论是可写（Writable）流还是可读（Readable）流，它们都会在内部对象中缓冲数据，这两个内部对象分别可以从 `_writableState.getBuffer()` 和 `_readableState.buffer` 中获取。

被缓冲的数据量取决于传递给构造函数的 `highWaterMark` 选项。

可读（Readable）流的滞留发生于实现调用 [stream.push(chunk)](./api_for_stream_implementors.md#push) 时。如果流的消费者没有调用 [stream.read()](./api_for_stream_consumers.md#read)，那么数据将待在内部队列中，直到被消费。

可写（Writable）流的滞留发生于用户反复调用 [stream.write(chunk)](./api_for_stream_consumers.md#write) 时，即便此时返回 `false`。

流（特别是其中的 [stream.pipe()](./api_for_stream_consumers.md#pipe) 方法）的初衷是将数据的滞留量限制在一个可接受的水平，以便不同速度的资源和目标不会淹没可用内存。


<div id="compatibility_with_older_Nodejs_versions" class="anchor"></div>
## 与 Node.js 早期版本的兼容性

在 v0.10 之前版本的 Node.js 中，可读（Readable）流的接口较为简单，同时功能和实用性也较弱。

- ['data'](./api_for_stream_consumers.md#readable_event_data) 事件会在开始时立即发生，而不会等你调用 [stream.read()](./api_for_stream_consumers.md#read) 方法。如果你需要进行某些 I/O 来决定如何处理数据，那么你只能将数据块储存到某种缓冲区中以防它们流失。

- [stream.pause()](./api_for_stream_consumers.md#pause) 方法仅供参考，不保证生效。意味着即便当流处于暂停状态时，你仍需要准备接收 ['data'](./api_for_stream_consumers.md#readable_event_data) 事件。

在 Node.js v0.10 中新增了 Readable 类。为了向后兼容考虑，可读（Readable）流会在添加了 ['data'](./api_for_stream_consumers.md#readable_event_data) 事件监听器或调用 [stream.resume()](./api_for_stream_consumers.md#resume) 方法时切换至“流动模式”。其效果是，即便你不使用新的 [stream.read()](./api_for_stream_consumers.md#read) 方法和 ['readable'](./api_for_stream_consumers.md#readable_event_readable) 事件，你也不必担心丢失 ['data'](./api_for_stream_consumers.md#readable_event_data) 事件产生的数据块。

大多数程序会维持正常功能。然而，会在下列条件下引入一种边界情况：

- 没有添加 ['data'](./api_for_stream_consumers.md#readable_event_data) 事件处理器。

- [stream.resume()](./api_for_stream_consumers.md#resume) 方法从未被调用。

- 流未被导流到任何可写目标。

例如，请留意以下代码：

```javascript
// WARNING!  BROKEN!
net.createServer((socket) => {

    // we add an 'end' method, but never consume the data
    socket.on('end', () => {
        // It will never get here.
        socket.end('I got your message (but didnt read it)\n');
    });

}).listen(1337);
```

在 Node.js v0.10 之前的版本中，传入消息数据会被简单地丢弃。然而在 Node.js v0.10 及之后的版本中，socket 会一直保持暂停。

在这种情况下的解决方法是调用 [stream.resume()](./api_for_stream_consumers.md#resume) 方法来开启数据流：

```javascript
// Workaround
net.createServer((socket) => {

    socket.on('end', () => {
        socket.end('I got your message (but didnt read it)\n');
    });

    // start the flow of data, discarding it.
    socket.resume();

}).listen(1337);
```

除了流动模式下新建可读（Readable）流以外，v0.10 之前风格的流可以通过 [stream.wrap()](./api_for_stream_consumers.md#wrap) 方法包装成 Readable 类。


<div id="object_mode" class="anchor"></div>
## 对象模式

通常情况下，流只操作字符串和 Buffer。

处于**对象模式**的流除了 Buffer 和字符串外还能读取普通的 JavaScript 值。

一个处于对象模式的可读（Readable）流调用 [stream.read(size)](./api_for_stream_consumers.md#read) 时总会返回单个项目，无论传入什么 `size` 参数。

一个处于对象模式的可写（Writable）流总是会忽略传给 [stream.write(data, encoding)](./api_for_stream_consumers.md#write) 的 `encoding` 参数。

特殊值 `null` 在对象模式流中依旧保持它的特殊性。也就说，对于对象模式的可读流，[stream.read()](./api_for_stream_consumers.md#read) 返回 `null` 意味着没有更多数据，同时 [stream.push(null)](./api_for_stream_implementors.md#push) 标志着流数据已到达末端（`EOF`）。

Node.js 核心不存在对象模式的流。这种设计只被某些用户态流式库所使用。这种模式只用于用户级流库。

你应该在你的流子类构造函数的选项对象中设置 `objectMode`。在流的过程中设置 `objectMode` 是不安全的。

对于双工（Duplex）流而言，`objectMode` 只可以在可读端和可写端分别通过 `readableObjectMode` 和 `writableObjectMode` 来设置。这些选项可以被用于在转换（Transform）流中实现解析器和串行器。

```javascript
const util = require('util');
const StringDecoder = require('string_decoder').StringDecoder;
const Transform = require('stream').Transform;
util.inherits(JSONParseStream, Transform);

// Gets \n-delimited JSON string data, and emits the parsed objects
function JSONParseStream() {
    if (!(this instanceof JSONParseStream))
        return new JSONParseStream();

    Transform.call(this, {
        readableObjectMode: true
    });

    this._buffer = '';
    this._decoder = new StringDecoder('utf8');
}

JSONParseStream.prototype._transform = function (chunk, encoding, cb) {
    this._buffer += this._decoder.write(chunk);
    // split on newlines
    var lines = this._buffer.split(/\r?\n/);
    // keep the last partial line buffered
    this._buffer = lines.pop();
    for (var l = 0; l < lines.length; l++) {
        var line = lines[l];
        try {
            var obj = JSON.parse(line);
        } catch (er) {
            this.emit('error', er);
            return;
        }
        // push the parsed object out to the readable consumer
        this.push(obj);
    }
    cb();
};

JSONParseStream.prototype._flush = function (cb) {
    // Just handle any leftover
    var rem = this._buffer.trim();
    if (rem) {
        try {
            var obj = JSON.parse(rem);
        } catch (er) {
            this.emit('error', er);
            return;
        }
        // push the parsed object out to the readable consumer
        this.push(obj);
    }
    cb();
};
```


<div id="read" class="anchor"></div>
## stream.read(0)

在某些情况下，你可能需要在不真正消费任何数据的情况下，触发底层可读流机制的刷新。在这种情况下，你可以调用 `stream.read(0)`，它总会返回 null。

如果内部读取缓冲低于 highWaterMark 水位线，并且流当前不在读取状态，那么调用 `stream.read(0)` 会触发一个低级 [stream._read()](./api_for_stream_implementors.md#_read) 调用。

虽然几乎没有必要这么做，但你可以在 Node.js 内部的某些地方看到它确实这么做了，尤其是在可读（Readable）流类的内部。


<div id="push" class="anchor"></div>
## stream.push('')

推入一个零字节字符串或 Buffer（当不在[对象模式](#object_mode)时）有一个有趣的副作用。因为*它*是一个对 [stream.push()](./api_for_stream_implementors.md#push) 的调用，它会结束 `reading` 过程。然而，它*没有*添加任何数据到可读缓冲中，所以没有东西可以被用户消费。

在极少数情况下，你当时没有提供任何数据，但你的流的消费者（或你的代码的其它部分）会通过调用 [stream.read(0)](./api_for_stream_consumers.md#read) 得知何时需要再次检查。在这中情况下，你*可以*调用 `stream.push('')`。

到目前为止，这个功能唯一一个使用场景是在 [tls.CryptoStream](../tls/class_CryptoStream.md#) 类中，但在 Node.js/io.js v1.0 中已被废弃。如果你发现你不得不使用 `stream.push('')`，请考虑另一种方式，因为几乎可以明确表明这是某种可怕的错误。