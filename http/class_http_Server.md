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

当建立一个新的 TCP 流时发出。`socket` 是 [net.Socket](../net/class_net_Socket.md#) 对象的一个类型。通常用户无需处理此事件。特别注意，采用协议解析器绑定 socket 的方式会使其不发出 `'readable'` 事件。也可以通过 `request.connection` 访问 `socket`。


## 'connect' 事件

`function (request, socket, head) { }`

每当客户端请求一个 http 的 `CONNECT` 方法时发出。如果未监听该事件，那么客户端请求 `CONNECT` 方法会使自身的连接关闭。

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

每当客户端请求一个 http 升级时发出。