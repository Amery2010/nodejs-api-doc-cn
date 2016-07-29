# net.Server类

* ['connection' 事件](#event_connection)
* ['listening' 事件](#event_listening)
* ['close' 事件](#event_close)
* ['error' 事件](#event_error)
* [server.maxConnections](#maxConnections)
* [server.connections](#connections)
* [server.listening](#listening)
* [server.getConnections(callback)](#getConnections)
* [server.listen(options[, callback])](#listen_options)
* [server.listen(path[, backlog][, callback])](#listen_path)
* [server.listen(port[, hostname][, backlog][, callback])](#listen_port)
* [server.listen(handle[, backlog][, callback])](#listen_handle)
* [server.address()](#address)
* [server.close([callback])](#close)
* [server.ref()](#ref)
* [server.unref()](#unref)

--------------------------------------------------


此类用于创建 TCP 或本地服务器。

`net.Server` 是一个带有以下事件的 [EventEmitter](../events/class_EventEmitter.md#)。


<div id="event_connection" class="anchor"></div>
## 'connection' 事件

* {net.Socket} 连接对象

当一个新的连接建立时发出。`socket` 是一个 `net.Socket` 实例。


<div id="event_listening" class="anchor"></div>
## 'listening' 事件

当服务器调用 `server.listen` 后被绑定时发出。


<div id="event_close" class="anchor"></div>
## 'close' 事件

当服务器关闭时发出。请注意，如果存在连接，不会发出此事件，直到所有的连接都结束了才发出。


<div id="event_error" class="anchor"></div>
## 'error' 事件

* {Error}

当发生错误时发出。['close'](#event_close) 事件将这个事件后直接调用，详见在 `server.listen` 讨论中的例子。