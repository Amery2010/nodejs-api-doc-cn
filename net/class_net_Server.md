# net.Server类

* ['connection' 事件](#connection-事件)
* ['listening' 事件](#listening-事件)
* ['close' 事件](#close-事件)
* ['error' 事件](#error-事件)
* [server.maxConnections](#servermaxConnections)
* [server.connections](#serverconnections)
* [server.listening](#serverlistening)
* [server.getConnections(callback)](#servergetConnectionscallback)
* [server.listen(options[, callback])](#serverlistenoptions-callback)
* [server.listen(path[, backlog][, callback])](#serverlistenpath-backlog-callback)
* [server.listen(port[, hostname][, backlog][, callback])](#serverlistenport-hostname-backlog-callback)
* [server.listen(handle[, backlog][, callback])](#serverlistenhandle-backlog-callback)
* [server.address()](#serveraddress)
* [server.close([callback])](#serverclosecallback)
* [server.ref()](#serverref)
* [server.unref()](#serverunref)

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

> 稳定度：0 - 已废弃：使用 [server.getConnections()](#servergetConnectionscallback)