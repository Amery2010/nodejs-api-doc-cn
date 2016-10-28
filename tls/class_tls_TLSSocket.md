# tls.TLSSocket类

* [new tls.TLSSocket(socket[, options])](#new-tlstlssocketsocket-options)
* ['secureConnect' 事件](#secureconnect-事件)
* ['OCSPResponse' 事件](#ocspresponse-事件)
* [tlsSocket.localAddress](#tlssocketlocaladdress)
* [tlsSocket.localPort](#tlssocketlocalport)
* [tlsSocket.remoteFamily](#tlssocketremotefamily)
* [tlsSocket.remoteAddress](#tlssocketremoteaddress)
* [tlsSocket.remotePort](#tlssocketremoteport)
* [tlsSocket.encrypted](#tlssocketencrypted)
* [tlsSocket.authorized](#tlssocketauthorized)
* [tlsSocket.authorizationError](#tlssocketauthorizationerror)
* [tlsSocket.address()](#tlssocketaddress)
* [tlsSocket.getTLSTicket()](#tlssocketgettlsticket)
* [tlsSocket.getPeerCertificate([ detailed ])](#tlssocketgetpeercertificatedetailed)
* [tlsSocket.getCipher()](#tlssocketgetcipher)
* [tlsSocket.getEphemeralKeyInfo()](#tlssocketgetephemeralkeyinfo)
* [tlsSocket.getProtocol()](#tlssocketgetprotocol)
* [tlsSocket.getSession()](#tlssocketgetsession)
* [tlsSocket.renegotiate(options, callback)](#tlssocketrenegotiateoptions-callback)
* [tlsSocket.setMaxSendFragment(size)](#tlssocketsetmaxsendfragmentsize)

--------------------------------------------------


这是一个包装过的 [net.Socket](../net/class_net_Socket.md#) 版本，对写入的数据透明加密并且都需要 TLS 协商。

这个实例实现了[全双工流](../stream/api_for_stream_implementors.md#class_Duplex)接口。它具有普通流所有的方法和事件。

方法返回 TLS 连接元（如，当连接打开时，[tls.TLSSocket.getPeerCertificate()](#tlssocketgetpeercertificatedetailed) 只会返回数据。）。


## new tls.TLSSocket(socket[, options])

从现有的 TCP 套接字构造一个新的 TLSSocket 对象。

`socket` 是一个 [net.Socket](../net/class_net_Socket.md#) 实例。

`options` 是可能包含以下属性的一个可选对象：

* `secureContext`：一个源自 [tls.createSecureContext()](./tls/tls.md#tlscreatesecurecontextoptions) 可选的 TLS 上下文对象。

* `isServer`：如果为 `true`，TLS 套接字将在服务器模式下被实例化。默认：`false`。

* `server`：一个可选的 [net.Server](../net/class_net_Server.md#) 实例。

* `requestCert`：可选，详见 [tls.createSecurePair()](./tls.md#tlscreatesecurepaircontext-isserver-requestcert-rejectunauthorized-options)。

* `rejectUnauthorized`：可选，详见 [tls.createSecurePair()](./tls.md#tlscreatesecurepaircontext-isserver-requestcert-rejectunauthorized-options)。

* `NPNProtocols`：可选，详见 [tls.createServer()](./tls.md#tlscreateserveroptions-secureconnectionlistener)。

* `ALPNProtocols`：可选，详见 [tls.createServer()](./tls.md#tlscreateserveroptions-secureconnectionlistener)。

* `SNICallback`：可选，详见 [tls.createServer()](./tls.md#tlscreateserveroptions-secureconnectionlistener)。

* `session`：可选，一个 `Buffer` 实例，包含一个 TLS 会话。

* `requestOCSP`：可选，如果为 `true`，OCSP 状态请求扩展将被添加到客户端问候并在建立安全通信之前的套接字上发出一个 `'OCSPResponse'` 事件。


## 'secureConnect' 事件

这个事件会在新连接的握手过程已成功完成后发出。监听时将忽略服务器证书是否已被授权调用。测试 `tlsSocket.authorized` 是否是服务器证书是由指定的 CA 之一签署的，是用户的责任。如果 `tlsSocket.authorized === false`，那么错误可以在 `tlsSocket.authorizationError` 中找到。同样，如果使用了 `ALPN` 或 `NPN`，可以用 `tlsSocket.alpnProtocol` 或 `tlsSocket.npnProtocol` 检查协商的协议。


## 'OCSPResponse' 事件

`function (response) { }`

如果设置了 `requestOCSP` 选项将触发此事件。`response` 是一个包含服务器 OCSP 响应的 `Buffer`。

传统上，`response` 是一个来自服务器的 CA 的签名对象，包含有关服务器证书吊销状态的信息。