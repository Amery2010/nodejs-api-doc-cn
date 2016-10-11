# http.Server类

* ['connection' 事件](#connection-事件)
* ['connect' 事件](#connect-事件)
* ['checkContinue' 事件](#checkcontinue-事件)
* ['request' 事件](#request-事件)
* ['upgrade' 事件](#upgrade-事件)
* ['close' 事件](#close-事件)
* ['clientError' 事件](#clienterror-事件)
* [server.listening](#serverlistening)
* [server.maxHeadersCount](#servermaxheaderscount)
* [server.timeout](#servertimeout)
* [server.listen(handle[, callback])](#serverlistenhandle-callback)
* [server.listen(path[, callback])](#serverlistenpath-callback)
* [server.listen(port[, hostname][, backlog][, callback])](#serverlistenport-hostname-backlog-callback)
* [server.close([callback])](#serverclosecallback)
* [server.setTimeout(msecs, callback)](#serversettimeoutmsecs-callback)

--------------------------------------------------


该类继承 [net.Server](../net/class_net_Server.md#)，并且具有以下附加的事件：


## 'connection' 事件

`function (socket) { }`

当建立一个新的 TCP 流时发出。`socket` 是 [net.Socket](../net/class_net_Socket.md#) 对象的一个类型。通常用户无需处理此事件。特别注意，采用协议解析器绑定套接字的方式会使其不发出 `'readable'` 事件。也可以通过 `request.connection` 访问 `socket`。


## 'connect' 事件

`function (request, socket, head) { }`

每当客户端请求一个 http 的 `CONNECT` 方法时发出。如果未监听该事件，那么这些请求 `CONNECT` 方法的客户端连接将会被关闭。

* `request` 是一个 http 请求的参数，与 request 事件中的相同。

* `socket` 是服务端与客户端之间的网络套接字。

* `head` 是一个 Buffer 的实例，隧道流的第一个包，该参数可能为空。

当发出该事件后，请求的套接字将不会有 `'data'` 事件监听器，也就是说你将需要绑定一个监听器到 `'data'` 事件，来处理在套接字上被发送到服务器的数据。


## 'checkContinue' 事件

`function (request, response) { }` 

每当收到带有 Expect: 100-continue 的 http 请求时发出。如果未监听该事件，服务器会适当地自动发送 100 Continue 响应。

处理该事件时，如果客户端应该继续发送请求主体则调 [response.writeContinue()](./class_http_ServerResponse.md#responsewriteContinue)，否则生成适当的 HTTP 响应（如，400 错误的请求）。

请注意，当这个事件发出并处理后，将不再发出 `'request'` 事件。


## 'request' 事件

`function (request, response) { }`

每次收到一个请求时发出。注意，可能存在每个连接有多个请求（在 `keep-alive` 的情况下）。`request` 是 [http.IncomingMessage](./class_http_IncomingMessage.md#) 的一个实例，`response` 是 [http.ServerResponse](./class_http_ServerResponse.md#) 的一个实例。


## 'upgrade' 事件

`function (request, socket, head) { }`

每当客户端请求一个 http 升级时发出。如果未监听该事件，那么这些请求升级的客户端连接将会被关闭。

* `request` 是一个 http 请求的参数，与 request 事件中的相同。

* `socket` 是服务端与客户端之间的网络套接字。

* `head` 是一个 Buffer 的实例，隧道流的第一个包，该参数可能为空。

当发出该事件后，请求的套接字将不会有 `'data'` 事件监听器，也就是说你将需要绑定一个监听器到 `'data'` 事件，来处理在套接字上被发送到服务器的数据。


## 'close' 事件

`function () { }`

当服务器关闭时发出。


## 'clientError' 事件

`function (exception, socket) { }`

如果客户端发出了一个 `'error'` 事件，那么它将在这里转发。

`socket` 是发生错误的 [net.Socket](../net/class_net_Socket.md#) 对象。


## server.listening

一个表示服务器是否监听连接的布尔值。


## server.maxHeadersCount

最大请求头数目限制, 默认为 1000个。如果设置为 0, 则表示不做任何限制。


## server.timeout

* {Number} 默认为 120000（2分钟）

一个套接字被判断为超时之前的闲置毫秒数。

注意套接字的超时逻辑在连接时设定，所以更改这个值只会影响*新*创建的连接，而不会影响到现有连接。

设置为 0 将阻止之后建立的连接的一切自动超时行为。


## server.listen(handle[, callback])

* `handle` {Object}

* `callback` {Function}

`handle` 对象可以被设置为 server 或 socket (任何以下划线开头的  _handle 成员)，或一个 `{fd: <n>}` 对象。

这将导致服务器使用指定的句柄接受连接，但它假设文件描述符或者句柄已经被绑定到了特定的端口或者域名套接字。

在 Windows 平台上不支持监听文件描述符。

该函数是异步的。最后的参数 `callback` 会被添加到 `'listening'` 事件的监听器中。参见 [net.Server.listen()](../net/class_net_Server.md#serverlistenhandle-backlog-callback)。

返回 `server`。


## server.listen(path[, callback])

启动一个 UNIX 套接字服务器监听给定 `path` 上的连接。

该函数是异步的。最后的参数 `callback` 会被添加到 `'listening'` 事件的监听器中。参见 [net.Server.listen(path)](../net/class_net_Server.md#serverlistenpath-backlog-callback)。


## server.listen(port[, hostname][, backlog][, callback])

开始在指定的 `port` 和 `hostname` 上接收连接。如果省略了 `hostname`，当 IPv6 可用时，该服务器会接收任何在 IPv6 地址（`::`）上的连接，否则便接收任何在 IPv4 地址（`0.0.0.0`）上的连接。

监听一个 UNIX 套接字，需要提供文件名而不是端口和主机名。

`backlog` 是等待连接队列的最大长度。实际长度由你的操作系统通过 `sysctl` 设置决定，比如 Linux 上的 `tcp_max_syn_backlog` 和 `somaxconn`。该参数的默认值是 511（不是 512）。

该函数是异步的。最后的参数 `callback` 会被添加到 `'listening'` 事件的监听器中。参见 [net.Server.listen(port)](../net/class_net_Server.md#serverlistenport-hostname-backlog-callback)。


## server.close([callback])

禁止服务端接收新的连接。详见 [net.Server.close()](../net/class_net_Server.md#serverclosecallback)。


## server.setTimeout(msecs, callback)

* `msecs` {Number}

* `callback` {Function}

为套接字设置超时值。如果一个超时发生，那么 Server 对象上会发出一个 `'timeout'` 事件，同时将该套接字作为参数传递。

如果在 Server 对象上有 `'timeout'` 事件监听器，那么它将被调用，而超时的套接字会作为参数传递给这个监听器。

默认情况下，服务器的超时时间是 2分钟，超时后套接字会被自动销毁。然而，如果你为该服务器的 `'timeout'` 事件分配了回调函数，那么你需要负责处理套接字的超时。

返回 `server`。