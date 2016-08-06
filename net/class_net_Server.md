# net.Server类

* ['connection' 事件](#connection-事件)
* ['listening' 事件](#listening-事件)
* ['close' 事件](#close-事件)
* ['error' 事件](#error-事件)
* [server.maxConnections](#servermaxConnections)
* [server.connections](#serverconnections)
* [server.listening](#serverlistening)
* [server.getConnections(callback)](#servergetConnectionscallback)
* [server.listen(port[, hostname][, backlog][, callback])](#serverlistenport-hostname-backlog-callback)
* [server.listen(path[, backlog][, callback])](#serverlistenpath-backlog-callback)
* [server.listen(handle[, backlog][, callback])](#serverlistenhandle-backlog-callback)
* [server.listen(options[, callback])](#serverlistenoptions-callback)
* [server.address()](#serveraddress)
* [server.close([callback])](#serverclosecallback)
* [server.unref()](#serverunref)
* [server.ref()](#serverref)

--------------------------------------------------


此类用于创建 TCP 或本地服务器。

`net.Server` 是一个带有以下事件的 [EventEmitter](../events/class_EventEmitter.md#)。


## 'connection' 事件

* {net.Socket} 连接对象

当一个新的连接建立时发出。`socket` 是一个 `net.Socket` 实例。


## 'listening' 事件

当服务器调用 `server.listen` 后被绑定时发出。


## 'close' 事件

当服务器关闭时发出。请注意，如果存在连接，不会发出此事件，直到所有的连接都结束了才发出。


## 'error' 事件

* {Error}

当发生错误时发出。['close'](#event_close) 事件将这个事件后直接调用，详见在 `server.listen` 讨论中的例子。


## server.maxConnections

当服务器的连接数变高时，设置此属性可以拒绝更多的连接。

一旦一个 socket 通过 [child_process.fork()](../child_process/asynchronous_process_creation.md#fork) 被发送到子进程时，不推荐使用此选项。


## server.connections

> 稳定度：0 - 已废弃：使用 [server.getConnections()](#servergetConnectionscallback) 替代。

服务器上的并行连接数。

当通过 [child_process.fork()](../child_process/asynchronous_process_creation.md#fork) 发送一个 socket 到子进程时，它会变成 `null`。要轮询派生进程并获得当前活动的连接号，请使用异步的 `server.getConnections` 代替。


## server.listen(port[, hostname][, backlog][, callback])

在指定的 `port` 和 `hostname` 上，开始接受连接。如果省略 `hostname`，当 `IPv6` 可用时，该服务器将接收任何 IPv6 地址（`::`）的连接，否则，接收任何 IPv4 地址（`0.0.0.0`）的连接。当端口值为零时，将分配一个随机端口。

`backlog` 正在连接队列的最大长度。实际长度会根据你的操作系统通过 sysctl 设置来定，例如在 linux 上的 `tcp_max_syn_backlog` 和 `somaxconn`。该参数的默认值是 `511`（不是 `512`）。

该函数是异步的。当服务器已经绑定，将发出 ['listening'](#listening-事件) 事件。最后的参数 `callback` 将会作为 ['listening'](#listening-事件) 事件的一个监听器。

有一个问题，一些用户在运行后得到了 `EADDRINUSE` 错误。这意味着，另一个服务器已经运行在了请求的端口上。处理的方法之一是等待一秒钟，然后重试。这可以这么做：

``` javascript
server.on('error', (e) => {
    if (e.code == 'EADDRINUSE') {
        console.log('Address in use, retrying...');
        setTimeout(() => {
            server.close();
            server.listen(PORT, HOST);
        }, 1000);
    }
});
```

（注意：Node.js 的所有 socket 都已经设置了 `SO_REUSEADDR`。）


## server.listen(path[, backlog][, callback])

* `path` {String}

* `backlog` {Number}

* `callback` {Function}

启动本地 socket 服务器监听指定 `path` 上的连接。

该函数是异步的。当服务器已经绑定，将发出 ['listening'](#listening-事件) 事件。最后的参数 `callback` 将会作为 ['listening'](#listening-事件) 事件的一个监听器。

在 UNIX 中，本地域通常称之为 UNIX 域。路径是一个文件系统路径名。它受到相同的命名约定和在文件创建时完成的权限检查，将在文件系统中可见，并将*持续到解除链接*。

在 Windows 中，本地域使用命名 pipe 来实现。该路径*必须*参考 `\\?\pipe\` 或 `\\.\pipe\` 条目。允许任何字符，但后者可能会做 pipe 名称的一些处理，如解析 `..` 序列。尽管出现，该 pipe 命名空间是平的。Pipe *不会持续下去*，当它们的最后参考被关闭时，它们将被移除。不要忘记 JavaScript 字符串转义 requirejs 路径用双反斜线来指定。例如：

``` javascript
net.createServer().listen(path.join('\\\\?\\pipe', process.cwd(), 'myctl'))
```

`backlog` 参数的行为类似于 [server.listen(port[, hostname][, backlog][, callback])](serverlistenport-hostname-backlog-callback)。


## server.listen(handle[, backlog][, callback])

* `handle` {Object}

* `backlog` {Number}

* `callback` {Function}

`handle` 对象既可以被设置为一个服务器，也可以被设置为一个 socket（任何具有的基础 `_handle` 任何成员）或一个 `{fd: <n>}` 对象。

这会使得服务器接受指定的句柄连接，并假设它是一个文件描述符或已经绑定到端口的句柄或域 socket。

在 Windows 上不支持监听文件描述符。

该函数是异步的。当服务器已经绑定，将发出 ['listening'](#listening-事件) 事件。最后的参数 `callback` 将会作为 ['listening'](#listening-事件) 事件的一个监听器。

`backlog` 参数的行为类似于 [server.listen(port[, hostname][, backlog][, callback])](serverlistenport-hostname-backlog-callback)。

``` javascript
server.listen({
    host: 'localhost',
    port: 80,
    exclusive: true
});
```


## server.listen(options[, callback])

* `options` {Object} - 需要。支持以下属性：

  - `port` {Number} - 可选。
  
  - `host` {String} - 可选。
  
  - `backlog` {Number} - 可选。
  
  - `path` {String} - 可选。
  
  - `exclusive` {Boolean} - 可选。
  
* `callback` - 可选。

`options` 的 `port`、`host` 和 `backlog` 属性以及可选的回调函数，它们的行为和在 [server.listen(port[, hostname][, backlog][, callback])](serverlistenport-hostname-backlog-callback) 中的调用相同。另外，`path` 选项可用于指定 UNIX socket。

如果 `exclusive` 是 `false`（默认），那么集群的工作进程将使用相同的基础句柄，允许连接共享处理业务。当 `exclusive` 是 `true` 时，不共享句柄，并试图端口共享结果会导致错误。监听专用端口上的例子如下所示。


## server.address()

返回绑定的地址，该地址名称族和服务器的端口由操作系统报告。当得到一个系统分配的地址时，用于查找哪个端口已经被分配。返回具有三个属性的对象，如 `{ port: 12346, family: 'IPv4', address: '127.0.0.1' }`。

示例：

``` javascript
var server = net.createServer((socket) => {
    socket.end('goodbye\n');
}).on('error', (err) => {
    // handle errors here
    throw err;
});

// grab a random port.
server.listen(() => {
    address = server.address();
    console.log('opened server on %j', address);
});
```

直到发生 ['listening'](#listening-事件) 事件前，不要调用 `server.address()`。


## server.close([callback])

停止服务器接受新的连接，并保持现有的连接。该函数是异步的，当所有的连接都结束并且服务器发生 ['close'](#close-事件) 事件时，服务器最终关闭。可选的 `callback` 在 `'close'` 事件发生时调用一次。不同于事件，当它被关闭时，如果服务器未打开，它将用 Error 作为其唯一的参数调用。


## server.unref()

如果这是在事件系统中唯一的活动服务器，在服务器上调用 `unref` 将允许程序退出。如果服务器已经 `unref`d，再次调用 `unref` 将没有效果。

返回 `server`。


## server.ref()

与 `unref` 相反，在一个之前 `unref`d 的服务器上调用 `ref`，*不会*让程序退出，如果它是唯一剩下的服务器（默认行为）。如果服务器已经 `ref`d，再次调用 `ref` 将没有效果。

返回 `server`。