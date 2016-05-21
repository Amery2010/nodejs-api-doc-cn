# ChildProcess类

* ['message' 事件](#event_message)
* ['disconnect' 事件](#event_disconnect)
* ['close' 事件](#event_close)
* ['exit' 事件](#event_exit)
* ['error' 事件](#event_error)
* [child.pid](#pid)
* [child.connected](#connected)
* [child.stdio](#stdio)
* [child.stdin](#stdin)
* [child.stdout](#stdout)
* [child.stderr](#stderr)
* [child.send(message[, sendHandle[, options]][, callback])](#send)
  - [例子：发送服务器对象](#sending_a_server_object)
  - [例子：发送 socket 对象](#sending_a_socket_object)
* [child.disconnect()](#disconnect)
* [child.kill([signal])](#kill)

--------------------------------------------------


`ChildProcess` 类是代表衍生子进程的 [EventEmitters](../events/class_EventEmitter.md#) 的实例。

不打算直接创建 `ChildProcess` 的实例。相反，要使用 [child_process.spawn()](./asynchronous_process_creation.md#spawn)、 [child_process.exec()](./asynchronous_process_creation.md#exec)、 [child_process.execFile()](./asynchronous_process_creation.md#execFile) 或 [child_process.fork()](./asynchronous_process_creation.md#fork) 方法创建 `ChildProcess` 实例。


<div id="event_message" class="anchor"></div>
## 'message' 事件

- `message` {Object} 一个已解析的 JSON 对象或原始值。

- `sendHandle` {Handle} 一个 [net.Socket](../net/class_net_Socket.md#) 或 [net.Server](../net/class_net_Server.md#) 对象，或是 `undefined`。

当一个子进程使用 `process.send()` 发送信息时会触发 `'message'` 事件。


<div id="event_disconnect" class="anchor"></div>
## 'disconnect' 事件

在父进程或子进程中调用 `ChildProcess.disconnect()` 后触发 `'disconnect'` 事件。在断开后它将不能再发送或接收信息，并且 `ChildProcess.connected` 属性会被设为 `false`。


<div id="event_close" class="anchor"></div>
## 'close' 事件

- `code` {Number} 如果子进程退出自身，该值会是退出码。

-  `signal` {String} 子进程被终止时的信号。

当子进程的 stdio 流被关闭时触发 `'close'` 事件。这与 `'exit'` 事件不同，因为多个进程可能共享相同的 stdio 流。


<div id="event_exit" class="anchor"></div>
## 'exit' 事件

- `code` {Number} 如果子进程退出自身，该值会是退出码。

-  `signal` {String} 子进程被终止时的信号。

当子进程结束时触发 `'exit'` 事件。如果进程退出，`code` 会是进程的最终退出码，否则会是 `null`。如果进程是由于收到的信号而终止的，`signal` 会是信号的字符串名称，否则会是 `null`。这两个将总有一个是非空的。

注意，当 `'exit'` 事件触发时，子进程的 stdio 流仍可能打开着。

另外，还要注意，Node.js 建立了 `SIGINT` 和 `SIGTERM` 的信号处理程序，并且 Node.js 进程因为收到这些信号，所以不会立即终止。相反，Node.js 将执行一个清理序列的操作然后重新引发处理信号。

详见 `waitpid(2)`。


<div id="event_error" class="anchor"></div>
## 'error' 事件

- `err` {Error} 该错误对象。

每当出现以下情况时触发 `'error'` 事件：

1. 该进程无法被衍生时；

2. 该进程无法被杀死时；

3. 向子进程发送信息失败时。

请注意，在一个错误发生后，`'exit'` 事件可能会也可能不会触发。如果你同时监听了 `'exit'` 和 `'error'` 事件，谨防处理函数被多次调用。

请同时参阅 [ChildProcess#kill()](#kill) 和 [ChildProcess#send()](#send)。


<div id="pid" class="anchor"></div>
## child.pid

- {Number} 整数

返回子进程的进程 id（PID）。

```javascript
const spawn = require('child_process').spawn;
const grep = spawn('grep', ['ssh']);

console.log(`Spawned child pid: ${grep.pid}`);
grep.stdin.end();
```


<div id="connected" class="anchor"></div>
## child.connected

- {Boolean} 在调用 `.disconnect` 后被设为 `false`。

`child.connected` 属性指示是否仍可以从一个子进程发送和接收消息。当 `child.connected` 是 `false` 时，它将不再能够发送或接收的消息。


<div id="stdio" class="anchor"></div>
## child.stdio

- {Array}

一个到子进程的管道的稀疏数组，对应着传给 [child_process.spawn()](./asynchronous_process_creation.md#spawn) 的选项中值被设为 `'pipe'` 的 [stdio](./asynchronous_process_creation.md#stdio)。请注意，与 `child.stdin`、 `child.stdout` 和 `child.stderr` 分别对应的 `child.stdio[0]`、 `child.stdio[1]` 和 `child.stdio[2]` 同样有效。

在下面的例子中，只有子进程的 fd `1`（stdout）被配置为 pipe，所以只有该父进程的 `child.stdio[1]` 是一个流，且在数组中的其他值都是 `null`。

```javascript
const assert = require('assert');
const fs = require('fs');
const child_process = require('child_process');

const child = child_process.spawn('ls', {
    stdio: [
      0, // Use parents stdin for child
      'pipe', // Pipe child's stdout to parent
      fs.openSync('err.out', 'w') // Direct child's stderr to a file
    ]
});

assert.equal(child.stdio[0], null);
assert.equal(child.stdio[0], child.stdin);

assert(child.stdout);
assert.equal(child.stdio[1], child.stdout);

assert.equal(child.stdio[2], null);
assert.equal(child.stdio[2], child.stderr);
```


<div id="stdin" class="anchor"></div>
## child.stdin

- {Stream}

一个代表子进程的 `stdin` 的 `Writable Stream`。

*注意，如果一个子进程等待读取所有的输入，子进程直到该流在通过 `end()` 关闭前不会继续。*

如果衍生的子进程的 `stdio[0]` 设置任何不是 `'pipe'` 的值，那么这会是 `undefined`。

`child.stdin` 是 `child.stdio[0]` 的一个别名。这两个属性都会指向相同的值。


<div id="stdout" class="anchor"></div>
## child.stdout

- {Stream}

一个代表子进程的 `stdout` 的 `Readable Stream`。

如果衍生的子进程的 `stdio[1]` 设置任何不是 `'pipe'` 的值，那么这会是 `undefined`。

`child.stdout` 是 `child.stdio[1]` 的一个别名。这两个属性都会指向相同的值。


<div id="stderr" class="anchor"></div>
## child.stderr

- {Stream}

一个代表子进程的 `stderr` 的 `Readable Stream`。

如果衍生的子进程的 `stdio[2]` 设置任何不是 `'pipe'` 的值，那么这会是 `undefined`。

`child.stderr` 是 `child.stdio[2]` 的一个别名。这两个属性都会指向相同的值。


<div id="send" class="anchor"></div>
## child.send(message[, sendHandle[, options]][, callback])

- `message` {Object}

- `sendHandle` {Handle}

- `options` {Object}

- `callback` {Function}

- 返回：{Boolean}

当在父进程和子进程之间建立了一个 IPC 通道时（例如，当使用 [child_process.fork()](./asynchronous_process_creation.md#fork) 时），该 `child.send()` 方法可以用于将消息发送到子进程。当该进程是一个 Node.js 实例时，该信息可以通过 `process.on('message')` 事件接收到。

例如，在父脚本：

```javascript
const cp = require('child_process');
const n = cp.fork(`${__dirname}/sub.js`);

n.on('message', (m) => {
    console.log('PARENT got message:', m);
});

n.send({
    hello: 'world'
});
```

然后是子进程脚本，`'sub.js'` 可能看上去像这样：

```javascript
process.on('message', (m) => {
    console.log('CHILD got message:', m);
});

process.send({
    foo: 'bar'
});
```

Node.js 中的子进程有自己的一个 `process.send()` 方法，该方法允许子进程将信息发送回父进程。

当发送的是 `{cmd: 'NODE_foo'}` 消息时，则是一个特例。所有在 `cmd` 属性里包含前缀为 `NODE_` 的属性被认为是预留给 Node.js 核心代码内部使用的，并且不会触发子进程的 `process.on('message')` 事件。相反，这种消息被用于触发 `process.on('internalMessage')` 事件，并且被 Node.js 内部消费。应用程序应避免使用这样的消息或监听 `'internalMessage'` 事件作为不通知该变化的依据。

可选的 `sendHandle` 参数可能被传给 `child.send()`，它是用于将一个 TCP 服务器或 socket 对象传给子进程。子进程会将收到的这个对象作为第二个参数传给注册在 `process.on('message')` 事件上的回调函数。

该 `options` 参数，如果存在的话，是用于发送某些类型的处理程序的参数对象。`options` 支持以下属性：

* `keepOpen` - 当传 `net.Socket` 时的一个可使用的 Boolean 值。当它为 `true` 时，socket 在发送进程中保持开启状态。默认为 `false`。

可选的 `callback` 是一个函数，它在信息发送后，但在子进程可能收到信息之前被调用。该函数被调用后只有一个参数：成功时是 `null`，或在失败时是一个 [Error](../errors/class_Error.md#) 对象。

如果没有提供 `callback` 函数，并且信息没被发送，一个 `'error'` 事件将被 `ChildProcess` 对象触发。这是有可能发生的，例如，当子进程已经退出时。

如果该通道已关闭或当未发送的信息积压超过阈值，使得它无法发送更多时，`child.send()` 将会返回 `false`。除此以外，该方法返回 `true`。该 `callback` 函数可用于实现流量控制。


<div id="sending_a_server_object" class="anchor"></div>
#### 例子：发送服务器对象

例如，`sendHandle` 参数可以用于将一个 TCP 服务器的处理程序传递给子进程，如下所示：

```javascript
const child = require('child_process').fork('child.js');

// Open up the server object and send the handle.
const server = require('net').createServer();
server.on('connection', (socket) => {
    socket.end('handled by parent');
});
server.listen(1337, () => {
    child.send('server', server);
});
```

那么子进程将会得到该服务器对象：

```javascript
process.on('message', (m, server) => {
    if (m === 'server') {
        server.on('connection', (socket) => {
            socket.end('handled by child');
        });
    }
});
```

一旦服务器现在是在父进程和子进程之间共享，那么一些连接可以由父进程处理，一些可以由子进程来处理。

虽然上面的例子使用了 `net` 模块创建了一个服务器，使用 `dgram` 模块创建的服务器使用完全相同的工作流程，不同的是它监听一个 `'message'` 事件而不是 `'connection'` 事件并使用 `server.bind` 替代 `server.listen`。但是，目前仅 UNIX 平台支持这一点。


<div id="sending_a_socket_object" class="anchor"></div>
#### 例子：发送 socket 对象

同样，`sendHandle` 参数可以用于将一个 socket 处理程序传递给子进程。以下的例子衍生了两个子进程分别用于处理 "normal" 连接或优先处理 "special" 连接：

```javascript
const normal = require('child_process').fork('child.js', ['normal']);
const special = require('child_process').fork('child.js', ['special']);

// Open up the server and send sockets to child
const server = require('net').createServer();
server.on('connection', (socket) => {

    // If this is special priority
    if (socket.remoteAddress === '74.125.127.100') {
        special.send('socket', socket);
        return;
    }
    // This is normal priority
    normal.send('socket', socket);
});
server.listen(1337);
```

该 `child.js` 会收到一个 socket 处理器作为第二个参数传递给事件回调函数：

```javascript
process.on('message', (m, socket) => {
    if (m === 'socket') {
        socket.end(`Request handled with ${process.argv[2]} priority`);
    }
});
```

一旦一个 socket 被传递给了子进程，父进程不再能够跟踪套接字何时被销毁。为了说明这一点，`.connections` 属性会变成 `null`。当发生这种情况时，建议不要使用 `.maxConnections`。

*注意，该函数内部使用 [JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 序列化 `message`。*


<div id="disconnect" class="anchor"></div>
## child.disconnect()

关闭父进程与子进程之间的 IPC 通道，一旦没有其他的连接使保持它活着，将允许子进程正常退出。在调用该方法后，分别在父进程和子进程上的 `child.connected` 和 `process.connected` 属性都会被设置为 `false`，并且它将不再能够在进程之间传递消息。

当正在接收的进程中没有消息时，将会触发 `'disconnect'` 事件。这经常在调用 `child.disconnect()` 后立即被触发。

请注意，当子进程是一个 Node.js 实例（例如，使用 [child_process.fork()](./asynchronous_process_creation.md#fork) 衍生）时，最好在子进程内部调用 `process.disconnect()` 方法来关闭 IPC 通道。


<div id="kill" class="anchor"></div>
## child.kill([signal])

- `signal` {String}

`child.kill()` 方法向子进程发送一个信号。如果没有给定参数，该进程将会发送 `'SIGTERM'` 信号。可以在 `signal(7)` 中查阅可用的信号列表。

```javascript
const spawn = require('child_process').spawn;
const grep = spawn('grep', ['ssh']);

grep.on('close', (code, signal) => {
    console.log(`child process terminated due to receipt of signal ${signal}`);
});

// Send SIGHUP to process
grep.kill('SIGHUP');
```

如果信号没有被送达，`ChildProcess` 对象可能会触发一个 `'error'` 事件。向一个已经退出的进程发送一个信号不是一个错误，但可能有无法预知的后果。特别是如果该进程的 id 已经被其他程序注册时，信号会被发送到该进程，反而它可能会有意想不到的结果。

请注意，当函数被调用 `kill` 时，发送到子进程处理的信号实际上可能没有终止该进程。

见 `kill(2)`，以供参考。

还要注意：当试图杀死他们的父进程时，子进程的子进程不会被终止。这可能发生在当在一个 shell 中运行一个新进程时或使用 `ChildProcess` 中的 `shell` 选项时，如本示例所示：

```javascript
'use strict';
const spawn = require('child_process').spawn;

let child = spawn('sh', ['-c',
  `node -e "setInterval(() => {
      console.log(process.pid + 'is alive')
    }, 500);"`
  ], {
    stdio: ['inherit', 'inherit', 'inherit']
});

setTimeout(() => {
    child.kill(); // does not terminate the node process in the shell
}, 2000);
```

