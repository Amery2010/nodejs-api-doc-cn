# 面向消费者的API

* [stream.Readable类](#class_Readable)
  - [readable事件](#readable_event_readable)
  - [data事件](#readable_event_data)
  - [end事件](#readable_event_end)
  - [close事件](#readable_event_close)
  - [error事件](#readable_event_error)
  - [readable.read([size])](#read)
  - [readable.setEncoding(encoding)](#setEncoding)
  - [readable.pipe(destination[, options])](#pipe)
  - [readable.unpipe([destination])](#unpipe)
  - [readable.unshift(chunk)](#unshift)
  - [readable.wrap(stream)](#wrap)
  - [readable.pause()](#pause)
  - [readable.resume()](#resume)
  - [readable.isPaused()](#isPaused)
* [stream.Writable类](#class_Writable)
  - [pipe事件](#writable_event_pipe)
  - [unpipe事件](#writable_event_unpipe)
  - [drain事件](#writable_event_drain)
  - [finish事件](#writable_event_finish)
  - [error事件](#writable_event_error)
  - [writable.write(chunk[, encoding][, callback])](#write)
  - [writable.setDefaultEncoding(encoding)](#setDefaultEncoding)
  - [writable.cork()](#cork)
  - [writable.uncork()](#uncork)
  - [writable.end([chunk][, encoding][, callback])](#end)
* [stream.Duplex类](#class_Duplex)
* [stream.Transform类](#class_Transform)

--------------------------------------------------


流可以是可读（[Readable](#class_Readable)）、可写（[Writable](#class_Writable)），或者兼具两者（[Duplex](#class_Duplex)，双工）的。

所有流都是 `EventEmitter` 的实例，但它们也有其它自定义的方法和属性，这取决于它们是 可读（Readable）、可写（Writable） 或 双工（Duplex）。

如果一个流既可读（Readable）也可写（Writable），那么它就实现了（流的）所有方法和事件。因此，[Duplex](#class_Duplex) 或 [Transform](#class_Transform) 流充分诠释了这些 API ，虽然它们的实现可能有所不同。

没有必要在你的程序里的消费者流中实现流接口。如果你*正在*自己的程序中实现流接口，请同时参阅[面向实现者的API](./api_for_stream_implementors.md#)

几乎所有 Node.js 程序，无论多么简单，都以某种方式使用了流。这里有一个使用流的 Node.js 程序的例子：

```javascript
const http = require('http');

var server = http.createServer((req, res) => {
    // req is an http.IncomingMessage, which is a Readable Stream
    // res is an http.ServerResponse, which is a Writable Stream

    var body = '';
    // we want to get the data as utf8 strings
    // If you don't set an encoding, then you'll get Buffer objects
    req.setEncoding('utf8');

    // Readable streams emit 'data' events once a listener is added
    req.on('data', (chunk) => {
        body += chunk;
    });

    // the end event tells you that you have entire body
    req.on('end', () => {
        try {
            var data = JSON.parse(body);
        } catch (er) {
            // uh oh!  bad json!
            res.statusCode = 400;
            return res.end(`error: ${er.message}`);
        }

        // write back something interesting to the user:
        res.write(typeof data);
        res.end();
    });
});

server.listen(1337);

// $ curl localhost:1337 -d '{}'
// object
// $ curl localhost:1337 -d '"foo"'
// string
// $ curl localhost:1337 -d 'not json'
// error: Unexpected token o
```


<div id="class_Readable" class="anchor"></div>
## stream.Readable类

可读（Readable）流接口是你正在读取的一个数据*来源*的抽象。换言之，数据*出自*一个可读（Readable）流。

一个可读（Readable）流在你表明你准备接收前将不会开始发出数据。

可读（Readable）流有两种“模式”：**流动模式**和**暂停模式**。当处于流动模式时，数据由底层系统读出，并尽可能快地提供给你的程序；当处于暂停模式时，你必须明确地调用 [stream.read()](#read) 来取出数据块。流开始处于暂停模式。

注意：如果没有绑定数据事件处理器，并且没有 [stream.pipe()](#pipe) 目标，同时流被切换到流动模式，那么数据将会流失。

你可以通过下面几种做法切换到流动模式：

* 添加一个 ['data' 事件](#readable_event_data)处理器来监听数据。

* 调用 [stream.resume()](#resume) 方法来明确开启数据流。

* 调用 [stream.pipe()](#pipe) 方法将数据发送到一个可写（[Writable](#class_Writable)）流。

你可以通过下面的一种做法切换回暂停模式：

* 如果没有导流目标，可以调用 [stream.pause()](#pause) 方法。

* 如果有导流目标，移除所有 ['data' 事件](#readable_event_data) 处理器，并通过调用 [stream.unpipe()](#unpipe) 方法移除所有导流目标。

请注意，为了向后兼容考虑，移除 ['data' 事件](#readable_event_data) 处理器*并不会*自动暂停流。同样的，当有导流目标时，调用 [stream.pause()](#pause) 并不能保证流在那些目标排空并请求更多数据时*维持*暂停状态。

可读（Readable）流的例子包括：

* [客户端上的 HTTP 响应](../http/class_http_IncomingMessage.md#)

* [服务器上的 HTTP 请求](../http/class_http_IncomingMessage.md#)

* [fs 读取流](../fs/class_fs_ReadStream.md#)

* [zlib 流](../zlib/)

* [crypto 流](../crypto/)

* [TCP 嵌套字](../net/class_net_Socket.md#)

* [子进程的 stdout 和 stderr](../child_process/class_ChildProcess.md#)

* [process.stdin](../process/process.md#stdin)


<div id="readable_event_readable" class="anchor"></div>
#### readable事件

当可以从流中读取数据块时，它就会触发一个 `'readable'` 事件。

在某些情况下，假如未准备好，监听一个 `'readable'` 事件会使一些数据从底层系统被读出到内部缓冲区中。

```javascript
var readable = getReadableStreamSomehow();
readable.on('readable', () => {
    // there is some data to read now
});
```

当内部缓冲区被排空后，一旦有更多数据可用时，会再次触发一个 `'readable'` 事件。

`'readable'` 事件不会在“流动”模式中触发的最后唯一一个例外是在流的末尾。

`'readable'` 事件表明流有新的信息：无论新的数据是可用的，还是已经到达流的末尾。在前者的情况下，[stream.read()](#read) 将会返回那些数据。在后者的情况下，[stream.read()](#read) 将会返回 `null` 。例如，在下面的例子中，`foo.txt` 是一个空文件：

```javascript
const fs = require('fs');
var rr = fs.createReadStream('foo.txt');
rr.on('readable', () => {
    console.log('readable:', rr.read());
});
rr.on('end', () => {
    console.log('end');
});
```

运行此脚本的输出：

```
$ node test.js
readable: null
end
```


<div id="readable_event_data" class="anchor"></div>
#### data事件

- `chunk` {Buffer} | {String} 数据块

绑定一个 `'data'` 事件监听器到一个未被明确暂停的流上，流会被切换到流动模式。数据只要可用就会被传递。

如果你想尽可能快地从流中取出所有的数据，这是最佳的方式。

```javascript
var readable = getReadableStreamSomehow();
readable.on('data', (chunk) => {
    console.log('got %d bytes of data', chunk.length);
});
```


<div id="readable_event_end" class="anchor"></div>
#### end事件

这个事件会在没有更多的数据可读时触发。

请注意，`'end'` 事件在数据被完全消费之前*不会被触发*。它可以在切换到流动模式后或直到你到达末端前通过不停地调用 [stream.read()](#read) 时被完成。

```javascript
var readable = getReadableStreamSomehow();
readable.on('data', (chunk) => {
    console.log('got %d bytes of data', chunk.length);
});
readable.on('end', () => {
    console.log('there will be no more data.');
});
```


<div id="readable_event_close" class="anchor"></div>
#### close事件

当流和底层数据源（比如，文件描述符）被关闭时触发。并不是所有流都会触发这个事件。这个事件表明没有更多的事件将被触发，并且不会做进一步的计算。

并不是所有的流都会触发 `'close'` 事件。


<div id="readable_event_error" class="anchor"></div>
#### error事件

- {Error}

在接收数据出错时触发。


<div id="read" class="anchor"></div>
#### readable.read([size])

- `size` {Number} 可选参数，指定要读取多少数据。

- 返回：{String} | {Buffer} | {Null}

`read()` 方法从内部缓冲区中拉取并返回一些数据。当没有数据可用，它会返回 `null`。

如果你传了一个 `size` 参数，那么它就会返回多少字节的数据。如果 `size` 字节不可用，那么它将返回 `null`，除非已经到了数据末端，在这种情况下，它将返回保留在缓冲区中的数据。

如果你没有指定 `size` 参数，那么它就会返回内部缓冲区中的所有数据。

该方法应该仅在暂停模式下被调用。在流动模式下，该方法会被自动调用直到内部缓冲区排空。

```javascript
var readable = getReadableStreamSomehow();
readable.on('readable', () => {
    var chunk;
    while (null !== (chunk = readable.read())) {
        console.log('got %d bytes of data', chunk.length);
    }
});
```

如果该方法返回了一个数据块，那么它也会触发 ['data'](#readable_event_data) 事件。

请注意，在 ['end' 事件](#readable_event_end)触发后调用 [stream.read([size])](#read) 将会返回 `null`，并且不会产生错误警告。


<div id="setEncoding" class="anchor"></div>
#### readable.setEncoding(encoding)

- `encoding` {String} 要使用的编码。

- 返回：`this`

调用此函数会使得流返回指定编码的字符串而不是 Buffer 对象。例如，如果你使用  `readable.setEncoding('utf8')`，那么输出数据会被作为 UTF-8 数据解析，并作为字符串返回。如果你  `readable.setEncoding('hex')`，那么数据会被编码成十六进制字符串格式。

该方法能妥善处理多字节字符，如果你直接取出 Buffer 并对它们调用 [buf.toString(encoding)](../buffer/class_Buffer.md#toString)，很可能会导致字节错位。如果你想要以字符串形式读取数据，请始终使用该方法。

你还可以使用 `readable.setEncoding(null)` 完全禁用任何编码。如果你在处理二进制数据或将大型的多字节字符串分成多块时，这种做法将非常有用。

```javascript
var readable = getReadableStreamSomehow();
readable.setEncoding('utf8');
readable.on('data', (chunk) => {
    assert.equal(typeof chunk, 'string');
    console.log('got %d characters of string data', chunk.length);
});
```


<div id="pipe" class="anchor"></div>
#### readable.pipe(destination[, options])

- `destination` {stream.Writable} 写入数据的目标

- `options` {Object} Pipe 选项
  - `end` {Boolean} 当读取结束时终止写入，默认为 `true`。

该方法从可读流中拉取所有数据，并写入到所提供的目标。该方法能自动控制流量以避免目标被快速读取的可读流所淹没。

可以安全地导流到多个目标。

```javascript
var readable = getReadableStreamSomehow();
var writable = fs.createWriteStream('file.txt');
// All the data from readable goes into 'file.txt'
readable.pipe(writable);
```

该函数返回目标的流，因此你可以建立像这样的导流链：

```javascript
var r = fs.createReadStream('file.txt');
var z = zlib.createGzip();
var w = fs.createWriteStream('file.txt.gz');
r.pipe(z).pipe(w);
```

例如，模拟 Unix 的 `cat` 命令：

```javascript
process.stdin.pipe(process.stdout);
```

默认情况下，当源数据流触发 ['end' 事件](#readable_event_end)时，目标的 [stream.end()](#end) 会被调用，因此 `destination` 不再可写。传入 `{end: false}` 作为 `options` 可以保持目标流的开启状态。

这让 `writer` 保持开启，因此最后可以写入 "Goodbye"。

```javascript
reader.pipe(writer, {
    end: false
});
reader.on('end', () => {
    writer.end('Goodbye\n');
});
```

请注意 [process.stderr](../process/process.md#stderr) 和 [process.stdout](../process/process.md#stdout) 在进程结束前都不会被关闭，无论是否指定选项。