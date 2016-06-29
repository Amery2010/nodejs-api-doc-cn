# 方法和属性

* ['setup' 事件](#event_setup)
* ['fork' 事件](#event_fork)
* ['online' 事件](#event_online)
* ['listening' 事件](#event_listening)
* ['message' 事件](#event_message)
* ['disconnect' 事件](#event_disconnect)
* ['exit' 事件](#event_exit)
* [cluster.settings](#settings)
* [cluster.isMaster](#isMaster)
* [cluster.isWorker](#isWorker)
* [cluster.worker](#worker)
* [cluster.workers](#workers)
* [cluster.schedulingPolicy](#schedulingPolicy)
* [cluster.setupMaster([settings])](#setupMaster)
* [cluster.fork([env])](#fork)
* [cluster.disconnect([callback])](#disconnect)

--------------------------------------------------


<div id="event_setup" class="anchor"></div>
## 'setup' 事件

* `settings` {Object}

每次调用 `.setupMaster()` 时都会触发。

在调用 `.setupMaster()` 时，`cluster.settings` 对象就是当时的 `settings` 对象并且仅供参考，因为可以在单个时钟周期内多次调用 `.setupMaster()`。

如果非常注重精度，请使用 `cluster.settings`。


<div id="event_fork" class="anchor"></div>
## 'fork' 事件

* `worker` {cluster.Worker}

当派生一个新的工作进程时，集群模块会触发一个 `'fork'` 事件。这可以用来记录工作进程的活动，并创建自己的超时时间。

``` javascript
var timeouts = [];

function errorMsg() {
    console.error('Something must be wrong with the connection ...');
}

cluster.on('fork', (worker) => {
    timeouts[worker.id] = setTimeout(errorMsg, 2000);
});

cluster.on('listening', (worker, address) => {
    clearTimeout(timeouts[worker.id]);
});

cluster.on('exit', (worker, code, signal) => {
    clearTimeout(timeouts[worker.id]);
    errorMsg();
});
```


<div id="event_online" class="anchor"></div>
## 'online' 事件

* `worker` {cluster.Worker}

在派生一个新的工作进程后，该工作进程应该用一个 online 信息作为回应。当主机收到了 online 信息，它会触发此事件。`'fork'` 和 `'online'` 的区别是：当主机派生了一个工作进程时，触发 `'fork'`；当工作进程运行时，触发 `'online'`。

``` javascript
cluster.on('online', (worker) => {
    console.log('Yay, the worker responded after it was forked');
});
```


<div id="event_listening" class="anchor"></div>
## 'listening' 事件

* `worker` {cluster.Worker}

* `address` {Object}

从一个工作进程中调用 `listen()` 后，当在服务器端触发 `'listening'` 事件时，在 `cluster` 主机上也会触发一个 `'listening'` 事件。

该事件处理器在执行时带有两个参数，`worker` 包含工作进程对象和 `address` 对象包含以下连接属性：`address`、`port` 和 `addressType`。如果该工作进程监听了多个地址，这时会非常有用。

``` javascript
cluster.on('listening', (worker, address) => {
    console.log(`A worker is now connected to ${address.address}:${address.port}`);
});
```

`addressType` 是以下之一：

* `4`（TCPv4）

* `6`（TCPv6）

* `-1`（unix domain socket）

* `"udp4"` 或 `"udp6"`（UDP v4 或 v6）


<div id="event_message" class="anchor"></div>
## 'message' 事件

* `worker` {cluster.Worker}

* `message` {Object}

当任何工作进程收到消息时触发。

详见[子进程 'message' 事件](../child_process/class_ChildProcess.md#event_message)。


<div id="event_disconnect" class="anchor"></div>
## 'disconnect' 事件

* `worker` {cluster.Worker}

在工作进程 IPC 通道断开后触发。它可以在工作进程正常退出、杀死或手动断开（如使用 `worker.disconnect()`）时发生。

在 `'disconnect'` 和 `'exit'` 事件之间可能有延迟。这些事件可以被用于检测进程是否卡在清理或处于长连接状态。

``` javascript
cluster.on('disconnect', (worker) => {
    console.log(`The worker #${worker.id} has disconnected`);
});
```


<div id="event_exit" class="anchor"></div>
## 'exit' 事件

* `worker` {cluster.Worker}

* `code` {Number} 它正常退出时的退出码。

* `signal` {String} 导致进程被杀死的信号名称（如 `'SIGHUP'`）。

当任何的工作进程死亡时，集群模块都会触发 `'exit'` 事件。

可以通过再次调用 `.fork()` 来重启工作进程。

``` javascript
cluster.on('exit', (worker, code, signal) => {
    console.log('worker %d died (%s). restarting...',
        worker.process.pid, signal || code);
    cluster.fork();
});
```

详见[子进程的 'exit' 事件](../child_process/class_ChildProcess.md#event_exit)。


<div id="settings" class="anchor"></div>
## cluster.settings

* {Object}

  - `execArgv` {Array} 传递给 Node.js 的可执行字符串参数列表。（默认 = `process.execArgv`）
  
  - `exec` {String} 工作进程文件的路径。（默认 = `process.argv[1]`）
  
  - `args` {Array} 传递给工作进程的字符串参数。（默认 = `process.argv.slice(2)`）
  
  - `silent` {Boolean} 是否将输出发送到父进程的 stdio。（默认 = `false`）
  
  - `uid` {Number} 设置进程的用户 id。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置进程的组 id。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
在调用 `.setupMaster()`（或 `.fork()`）后，设置对象会包含该设置，包括默认值。

它被设置后便不可更改，因为 `.setupMaster()` 只能被调用一次。


<div id="isMaster" class="anchor"></div>
## cluster.isMaster

* {Boolean}

如果该进程是主进程，则返回 `true`。这由 `process.env.NODE_UNIQUE_ID` 决定。如果 `process.env.NODE_UNIQUE_ID` 是 `undefined`，那么 `isMaster` 就是 `true`。


<div id="isWorker" class="anchor"></div>
## cluster.isWorker

* {Boolean}

如果该进程不是主进程，则返回 `true`。（它与 `cluster.isMaster` 相反）


<div id="worker" class="anchor"></div>
## cluster.worker

* {Object}

指向当前的工作进程对象。在主进程中不可用。

``` javascript
const cluster = require('cluster');

if (cluster.isMaster) {
    console.log('I am master');
    cluster.fork();
    cluster.fork();
} else if (cluster.isWorker) {
    console.log(`I am worker #${cluster.worker.id}`);
}
```


<div id="workers" class="anchor"></div>
## cluster.workers

* {Object}

存储着活跃的工作进程对象的散列，键为 `id` 字段。它使得遍历工作进程变得容易。只在主进程中可用。

工作进程在已断开连接*并退出*后，从 `cluster.workers` 中移除。这两个事件之间的顺序不能预先确定。然而，它可以保证在 `'disconnect'` 或 `'exit'` 事件触发前从 `cluster.workers` 列表中除去。

``` javascript
// Go through all workers
function eachWorker(callback) {
    for (var id in cluster.workers) {
        callback(cluster.workers[id]);
    }
}
eachWorker((worker) => {
    worker.send('big announcement to all workers');
});
```

如果你希望通过通信信道来引用一个工作进程，使用工作进程的唯一 id 是找到该工作进程的最简单方式。

``` javascript
socket.on('data', (id) => {
    var worker = cluster.workers[id];
});
```


<div id="schedulingPolicy" class="anchor"></div>
## cluster.schedulingPolicy

调度策略，`cluster.SCHED_RR` 表示轮流制，`cluster.SCHED_NONE` 表示交由操作系统处理。这是一个全局设置，并且一旦你派生了第一个工作进程或调用了 `cluster.setupMaster()` 后便不可更改。

`SCHED_RR` 是除 Windows 外所有操作系统的默认方式。只要 `libuv` 能够有效地分配 IOCP 句柄并且不产生巨大的性能损失，Windows 也将更改为 `SCHED_RR` 方式。

`cluster.schedulingPolicy` 同样也可以通过 `NODE_CLUSTER_SCHED_POLICY` 环境变量进行设置。有效值为 `"rr"` 和 `"none"`。


<div id="setupMaster" class="anchor"></div>
## cluster.setupMaster([settings])

* `settings` {Object}

  - `exec` {String} 工作进程文件的路径。（默认 = `process.argv[1]`）
  
  - `args` {Array} 传递给工作进程的字符串参数。（默认 = `process.argv.slice(2)`）
  
  - `silent` {Boolean} 是否将输出发送到父进程的 stdio。（默认 = `false`）
  
`setupMaster` 被用于改变 `'fork'` 的默认行为。一旦调用，该设置会成为当前的 `cluster.settings` 中相关的值。

注意：

* 任何的设置变更只影响之后调用的 `.fork()` 并且不影响已经运行的工作进程。

* 工作进程的*唯一*属性不能通过 `.setupMaster()` 传给 `.fork()` 的 `env` 进行设置。

* 以上的默认值只适用于第一次调用，最后一次的调用默认值是当时调用 `cluster.setupMaster()` 时的当前值。

例子：

``` javascript
const cluster = require('cluster');
cluster.setupMaster({
    exec: 'worker.js',
    args: ['--use', 'https'],
    silent: true
});
cluster.fork(); // https worker
cluster.setupMaster({
    exec: 'worker.js',
    args: ['--use', 'http']
});
cluster.fork(); // http worker
```


<div id="fork" class="anchor"></div>
## cluster.fork([env])

* `env` {Object} 添加到工作进程环境中的键值对。

* 返回 {cluster.Worker}

衍生一个新的工作进程。

这只能在主进程中调用。


<div id="disconnect" class="anchor"></div>
## cluster.disconnect([callback])

* `callback` {Function} 当所有的工作进程断开并关闭句柄时调用。

会在 `cluster.workers` 中的每个工作进程中调用 `.disconnect()`。

当他们断开所有内部句柄时会被关闭，如果不需要等待其他事件时，可以让主进程优雅地退出。

该方法接受一个可选的回调参数，在结束后调用。

这只能在主进程中调用。