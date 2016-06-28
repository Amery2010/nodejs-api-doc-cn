# 方法和属性

* ['setup' 事件](#event_setup)
* ['fork' 事件](#event_fork)
* ['online' 事件](#event_online)
* ['listening' 事件](#event_listening)
* ['message' 事件](#event_message)
* ['disconnect' 事件](#event_disconnect)
* ['exit' 事件](#event_exit)
* [cluster.settings](#settings)
* [cluster.setupMaster([settings])](#setupMaster)
* [cluster.fork([env])](#fork)
* [cluster.worker](#worker)
* [cluster.workers](#workers)
* [cluster.schedulingPolicy](#schedulingPolicy)
* [cluster.isMaster](#isMaster)
* [cluster.isWorker](#isWorker)
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

当衍生一个新的工作进程时，集群模块会触发一个 `'fork'` 事件。这可以用来记录工作进程的活动，并创建自己的超时时间。

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

在衍生一个新的工作进程后，该工作进程应该用一个 online 信息作为回应。当主机收到了 online 信息，它会触发此事件。`'fork'` 和 `'online'` 的区别是：当主机衍生了一个工作进程时，触发 `'fork'`；当工作进程运行时，触发 `'online'`。

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
