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
