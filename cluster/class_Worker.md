# Worker类

* ['online' 事件](#event_online)
* ['listening' 事件](#event_listening)
* ['message' 事件](#event_message)
* ['disconnect' 事件](#event_disconnect)
* ['exit' 事件](#event_exit)
* ['error' 事件](#event_error)
* [worker.id](#id)
* [worker.process](#process)
* [worker.suicide](#suicide)
* [worker.send(message[, sendHandle][, callback])](#send)
* [worker.disconnect()](#disconnect)
* [worker.kill([signal='SIGTERM'])](#kill)
* [worker.isConnected()](#isConnected)
* [worker.isDead()](#isDead)

--------------------------------------------------


<div id="event_online" class="anchor"></div>
## 'online' 事件

类似于 `cluster.on('online')` 事件，但指定为该工作进程。

``` javascript
cluster.fork().on('online', () => {
    // Worker is online
});
```

它不是在工作进程上触发的。


<div id="event_listening" class="anchor"></div>
## 'listening' 事件

* `address` {Object}

类似于 `cluster.on('listening')` 事件，但指定为该工作进程。

``` javascript
cluster.fork().on('listening', () => {
    // Worker is listening
});
```

它不是在工作进程上触发的。


<div id="event_message" class="anchor"></div>
## 'message' 事件

* `message` {Object}

类似于 `cluster.on('message')` 事件，但指定为该工作进程。

此事件与 [child_process.fork()](../child_process/asynchronous_process_creation.md#fork) 上提供的同名事件相同。

在工作进程中，你也可以使用 `process.on('message')`。

作为示例，有一个集群在主进程中使用消息系统保持对请求数量的计数：

``` javascript
const cluster = require('cluster');
const http = require('http');

if (cluster.isMaster) {

    // Keep track of http requests
    var numReqs = 0;
    setInterval(() => {
        console.log('numReqs =', numReqs);
    }, 1000);

    // Count requests
    function messageHandler(msg) {
        if (msg.cmd && msg.cmd == 'notifyRequest') {
            numReqs += 1;
        }
    }

    // Start workers and listen for messages containing notifyRequest
    const numCPUs = require('os').cpus().length;
    for (var i = 0; i < numCPUs; i++) {
        cluster.fork();
    }

    Object.keys(cluster.workers).forEach((id) => {
        cluster.workers[id].on('message', messageHandler);
    });

} else {

    // Worker processes have a http server.
    http.Server((req, res) => {
        res.writeHead(200);
        res.end('hello world\n');

        // notify master about the request
        process.send({
            cmd: 'notifyRequest'
        });
    }).listen(8000);
}
```


<div id="event_disconnect" class="anchor"></div>
## 'disconnect' 事件

类似于 `cluster.on('disconnect')` 事件，但指定为该工作进程。

``` javascript
cluster.fork().on('disconnect', () => {
    // Worker has disconnected
});
```


<div id="event_exit" class="anchor"></div>
## 'exit' 事件

* `code` {Number} 在正常退出时的退出码。

* `signal` {String} 导致进程被杀死的信号名称（如，`'SIGHUP'`）。

类似于 `cluster.on('exit')` 事件，但指定为该工作进程。

``` javascript
const worker = cluster.fork();
worker.on('exit', (code, signal) => {
    if (signal) {
        console.log(`worker was killed by signal: ${signal}`);
    } else if (code !== 0) {
        console.log(`worker exited with error code: ${code}`);
    } else {
        console.log('worker success!');
    }
});
```


<div id="event_error" class="anchor"></div>
## 'error' 事件

此事件与 [child_process.fork()](../child_process/asynchronous_process_creation.md#fork) 上提供的同名事件相同。

在工作进程中，你也可以使用 `process.on('error')`。


<div id="id" class="anchor"></div>
## worker.id

* {Number}

每个新的工作进程都被赋予它自己唯一的 ID，这个 ID 存储在 `id` 上。

工作进程还活着时，这个就是它在 ` cluster.workers` 中的索引键值。


<div id="process" class="anchor"></div>
## worker.process

* {ChildProcess}

所有的工作进程都是通过 [child_process.fork()](../child_process/asynchronous_process_creation.md#fork) 创建的，从该函数返回的对象被存储为 `.process`。在工作进程中，全局的 `process` 被存储。

详见：[子进程模块](../child_process/asynchronous_process_creation.md#fork)。

请注意，如果 `'disconnect'` 事件发生在 `process` 上，并且 `.suicide` 不是 `true`，则工作进程会调用 `process.exit(0)`。这可以防止意外断开。


<div id="suicide" class="anchor"></div>
## worker.suicide

* {Boolean}

通过调用 `.kill()` 或 `.disconnect()` 设置，直到那时变成 `undefined`。

`worker.suicide` 的布尔值可以让你区分自行退出和意外退出，主进程可以选择不重新衍生基于该值的工作进程。

``` javascript
cluster.on('exit', (worker, code, signal) => {
    if (worker.suicide === true) {
        console.log('Oh, it was just suicide\' – no need to worry').
    }
});

// kill worker
worker.kill();
```


<div id="send" class="anchor"></div>
## worker.send(message[, sendHandle][, callback])

* `message` {Object}

* `sendHandle` {Handle}

* `callback` {Function}

* 返回：`Boolean`

发送一个消息到一个工作进程或主进程，处理句柄是可选的。

在主进程中，它发送一个消息到一个指定工作进程上。它与 [ChildProcess.send()](../child_process/class_ChildProcess.md#send) 相同。

在工作进程中，它发送消息到主进程。它与 `process.send()` 相同。

这个例子会回复所有来自主进程的消息：

``` javascript
if (cluster.isMaster) {
    var worker = cluster.fork();
    worker.send('hi there');

} else if (cluster.isWorker) {
    process.on('message', (msg) => {
        process.send(msg);
    });
}
```


<div id="disconnect" class="anchor"></div>
## worker.disconnect()

在工作进程中，该函数会关闭所有的服务器，在那些服务器上等待 `'close'` 事件，然后断开 IPC 通道。

在主进程中，会发送一个内部消息到工作进程，使其自身调用 `.disconnect()`。

使得 `.suicide` 被设置。

注意，一个服务器被关闭后，它将不再接受新的连接，但连接可能会被其他任何正在监听的工作进程所接收。现有的连接将被允许正常关闭。当不存在连接时，详见 [server.close()](../net/net.md#close)，到工作进程的 IPC 通道会被关闭，且允许其优雅地死去。

以上*仅*适用于服务器的连接，客户端连接不会通过工作进程自动关闭，并且断开不会等到他们退出之前才关闭。

请注意，在一个工作进程中，存在 `process.disconnect`，但它不是这个函数，它是 [disconnect](../process/process.md#disconnect)。

因为长期活着的服务器连接可能会阻止工作进程断开，可能对它发送一个消息会很有用，所以应用指定动作可用于关闭它们。它同样对实现超时有用，如果在一段时间后没有触发 `'disconnect'` 事件，杀死该工作进程。


``` javascript
if (cluster.isMaster) {
    var worker = cluster.fork();
    var timeout;

    worker.on('listening', (address) => {
        worker.send('shutdown');
        worker.disconnect();
        timeout = setTimeout(() => {
            worker.kill();
        }, 2000);
    });

    worker.on('disconnect', () => {
        clearTimeout(timeout);
    });

} else if (cluster.isWorker) {
    const net = require('net');
    var server = net.createServer((socket) => {
        // connections never end
    });

    server.listen(8000);

    process.on('message', (msg) => {
        if (msg === 'shutdown') {
            // initiate graceful close of any connections to server
        }
    });
}
```


<div id="kill" class="anchor"></div>
## worker.kill([signal='SIGTERM'])

* `signal` {String} 发送给工作进程的 kill 信号名称。

该函数会杀死工作进程。在主进程中，它通过断开 `worker.process` 做到这一点，并且一旦断开，使用 `signal` 杀死进程。在工作进程中，它通过断开信道做到这一点，并在那时使用代码 `0` 退出。 

使得 `.suicide` 被设置。

该方法为了向下兼容作为 `worker.destroy()` 的别名。

请注意，在工作进程中，存在 `process.kill()`，但它不是这个函数，它是 [kill](../process/process.md#kill)。


<div id="isConnected" class="anchor"></div>
## worker.isConnected()

如果工作进程通过它的 IPC 通道连接到了它的主进程，那么这个函数返回 `true`，否则 `false`。一个工作进程在被创建后连接到它的主进程。它在触发 `'disconnect'` 事件后断开。


<div id="isDead" class="anchor"></div>
## worker.isDead()

如果工作进程已终止（无论是正常退出还是被信号关闭），这个函数返回 `true`，否则它返回 `false`。