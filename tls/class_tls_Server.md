# tls.Server类

* ['secureConnection' 事件](#secureconnection-事件)
* ['newSession' 事件](#newsession-事件)
* ['resumeSession' 事件](#resumesession-事件)
* ['OCSPRequest' 事件](#ocsprequest-事件)
* ['clientError' 事件](#clienterror-事件)
* [server.connections](#serverconnections)
* [server.maxConnections](#servermaxconnections)
* [server.listen(port[, hostname][, callback])](#serverlistenport-hostname-callback)
* [server.addContext(hostname, context)](#serveraddcontexthostname-context)
* [server.setTicketKeys(keys)](#serversetticketkeyskeys)
* [server.getTicketKeys()](#servergetticketkeys)
* [server.close([callback])](#serverclosecallback)
* [server.address()](#serveraddress)

--------------------------------------------------

这个类是 [net.Server]() 的子类，并且有着相同的方法。而不是只接受原始的TCP连接，它接收使用 TLS 或 SSL 加密的连接。


## 'secureConnection' 事件

`function (tlsSocket) {}`

在一个新的连接握手过程已成功完成后发出此事件。该参数是一个 [tls.TLSSocket](./class_tls_TLSSocket.md#) 实例并且拥有所有的公共流方法和事件。

`socket.authorized` 是一个布尔值，这表明客户端是否已被提供的证书颁发机构的服务器验证。如果 `socket.authorized` 是 `false`，那么 `socket.authorizationError` 被设定为描述授权如何失败。隐晦但值得一提：这取决于 TLS 服务器的设置，未经授权的连接可能被接受。

`socket.npnProtocol` 是包含所选 NPN 协议的字符串，同时 `socket.alpnProtocol` 是包含所选 ALPN 协议的字符串。当同时收到这两个 NPN 和 ALPN 扩展时，ALPN 优先于 NPN 并且 ALPN 将选择下一个协议。当 ALPN 没有已选择的协议时，这会返回 `false`。

`socket.servername` 是含有 SNI 请求的服务器名称的字符串。


## 'newSession' 事件

`function (sessionId, sessionData, callback) { }`

在建立一个 TLS 会话时发出。可以用来在外部存储中存储会话。最终必须调用 `callback`，否则没有数据将被发送或从安全连接中接收。

**注意**：添加这个事件监听器只对这之后建立的连接有影响。


## 'resumeSession' 事件

`function (sessionId, callback) { }`

当客户想要恢复以前的 TLS 会话时发出。该事件监听器可以使用给定的 `sessionId` 在外部存储中执行查找。并在完成时调用一次 `callback(null, sessionData)`。如果无法恢复会话（即，不存在于存储中），可能会调用 `callback(null, null)`。调用 `callback(err)` 将终止传入的连接并销毁套接字。

**注意**：添加这个事件监听器只对这之后建立的连接有影响。

下面是使用 TLS 会话恢复的例子：

``` javascript
var tlsSessionStore = {};
server.on('newSession', (id, data, cb) => {
    tlsSessionStore[id.toString('hex')] = data;
    cb();
});
server.on('resumeSession', (id, cb) => {
    cb(null, tlsSessionStore[id.toString('hex')] || null);
});
```


## 'OCSPRequest' 事件

`function (certificate, issuer, callback) { }`

当客户端发送一个证书状态请求时发出。可以解析服务器的当前证书来获得 OCSP URL 和证书编号；在获得一个 OCSP 响应之后，之后调用 `callback(null, resp)`，其中 `resp` 是一个 `Buffer` 实例。`certificate` 和 `issuer` 都是 `Buffer` 形式的主要和发行人证书的 DER 表示。它们可被用于获得 OCSP 证书的 ID 和 OCSP 端点 URL。

另外，可以调用 `callback(null, null)`，这意味着没有 OCSP 响应。

调用 `callback(err)`，会导致调用 `socket.destroy(err)`。

典型流程：

1. 客户端连接到服务器并向它发送一个 `'OCSPRequest'`（通过 ClientHello 上的状态信息扩展）。

2. 服务器收到请求并调用 `'OCSPRequest'` 事件监听器，如果存在的话。

3. 服务器从 `certificate` 或 `issuer` 提取 OCSP URL 并向 CA 执行 [OCSP 请求](https://en.wikipedia.org/wiki/OCSP_stapling)。

4. 服务器从 CA 接收 `OCSPResponse` 并通过 `callback` 参数将其发送回客户端。

5. 客户端验证响应，要么销毁套接字，要么执行握手。

**注意**：如果证书是自签名或发行人不在根证书列表中时，`issuer` 不能是 `null`。（发行人可以通过 `ca` 选项提供。）

**注意**：添加这个事件监听器只对这之后建立的连接有影响。

**注意**：像 [asn1.js](https://npmjs.org/package/asn1.js) 这样的 npm 模块可用于解析证书。


## 'clientError' 事件

`function (exception, tlsSocket) { }`

当一个客户端连接在建立安全连接之前发出一个 `'error'` 事件时，它将在这里转发。

`tlsSocket` 是源自 [tls.TLSSocket](./class_tls_TLSSocket.md#) 的错误。


## server.connections

在服务器上的并行连接数。


## server.maxConnections

将此属性设置为，当服务器的连接数超过指定的阈值时拒绝连接。


## server.listen(port[, hostname][, callback])

在指定的 `port` 和 `hostname` 上开始接受连接。如果省略 `hostname`，当 IPv6 可用时，服务器将接受任何 IPv6 地址（`::`），否则将接收任何 IPv4 地址（`0.0.0.0`）。零端口值将被分配一个随机端口。

这个函数是异步的。最后的参数 `callback` 在服务器被绑定时调用。

详见 `net.Server` 了解更多详情。


## server.addContext(hostname, context)

如果客户端请求的主机名 SNI 所提供的 `hostname` 匹配（可以使用通配符），添加将被使用的安全上下文。`context` 可以包含 `key`、`cert`、`ca` 或来自 [tls.createSecureContext()](./tls.md#tlscreatesecurecontextoptions) 中 `options` 参数的任何其他属性。


## server.setTicketKeys(keys)

升级用于[ TLS 会话凭证](https://www.ietf.org/rfc/rfc5077.txt) 加密/解密的密钥。

**注意**：buffer 应该是 48 个字节长度。详见在 [tls.createServer](./tls.md#tlscreateserveroptions-secureconnectionlistener) 中的 `ticketKeys` 选项了解更多如何使用的信息。

**注意**：该变化仅对之后的服务器连接有效。现有的或目前正在等待的服务器的连接将使用之前的密钥。


## server.getTicketKeys()

返回一个保存有目前用于[ TLS 会话凭证](https://www.ietf.org/rfc/rfc5077.txt) 加密/解密的密钥的 `Buffer` 实例。


## server.close([callback])

服务器停止接受新的连接。该函数是异步的，当服务器发出一个 `'close'` 事件时，该服务器最终关闭。你可以传递一个可选的回调函数到 `'close'` 事件的监听器中。


## server.address()

返回由操作系统所报告的绑定地址、地址族名称和服务器端口号。详见 [net.Server.address()](../net/class_net_Server.md#serveraddress) 了解更多信息。