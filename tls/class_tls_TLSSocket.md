# tls.TLSSocket类

* [new tls.TLSSocket(socket[, options])](#new-tlstlssocketsocket-options)
* ['secureConnect' 事件](#secureconnect-事件)
* ['OCSPResponse' 事件](#ocspresponse-事件)
* [tlsSocket.localAddress](#tlssocketlocaladdress)
* [tlsSocket.localPort](#tlssocketlocalport)
* [tlsSocket.remoteFamily](#tlssocketremotefamily)
* [tlsSocket.remoteAddress](#tlssocketremoteaddress)
* [tlsSocket.remotePort](#tlssocketremoteport)
* [tlsSocket.authorized](#tlssocketauthorized)
* [tlsSocket.authorizationError](#tlssocketauthorizationerror)
* [tlsSocket.encrypted](#tlssocketencrypted)
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


## tlsSocket.localAddress

本地 IP 地址的字符串表示。


## tlsSocket.localPort

本地端口的数字表示。


## tlsSocket.remoteFamily

远程 IP 族的字符串表示。`'IPv4'` 或 `'IPv6'`。


## tlsSocket.remoteAddress

远程 IP 地址的字符串表示。例如，`'74.125.127.100'` 或 `'2001:4860:a005::68'`。


## tlsSocket.remotePort

远程端口的数字表示。例如，`443`。


## tlsSocket.authorized

布尔值，如果对方的证书是由指定的 CA 之一签署的，则为 `true`，否则为 `false`。


## tlsSocket.authorizationError

为什么对方的证书未能通过验证的原因。这个属性只在 `tlsSocket.authorized === false` 时可用。


## tlsSocket.encrypted

静态布尔值，总是 `true`，可用来区分那些常规的 TLS 套接字。


## tlsSocket.address()

返回由操作系统所报告的绑定地址、地址族名称和服务器端口号。返回一个包含三个属性的对象，即，`{ port: 12346, family: 'IPv4', address: '127.0.0.1' }`。


## tlsSocket.getTLSTicket()

**注意**：仅适用于客户端的 TLS 套接字。有用仅用于调试，用于会话重用提供给 [tls.connect()](./tls.md#tlsconnectoptions-callback) 的 `session` 选项。

返回 TLS 会话凭证，如果没有进行协商，则返回 `undefined`。


## tlsSocket.getPeerCertificate([ detailed ])

返回表示对等体的证书对象。返回的对象有一些相对于证书字段的属性。如果 `detailed` 是 `true`，将会返回完整的带有 `issuer` 属性的链，如果为 `false`，只会返回不带 `issuer` 属性的顶级证书。

示例：

``` javascript
{
    subject: {
        C: 'UK',
        ST: 'Acknack Ltd',
        L: 'Rhys Jones',
        O: 'node.js',
        OU: 'Test TLS Certificate',
        CN: 'localhost'
    },
    issuerInfo: {
        C: 'UK',
        ST: 'Acknack Ltd',
        L: 'Rhys Jones',
        O: 'node.js',
        OU: 'Test TLS Certificate',
        CN: 'localhost'
    },
    issuer: {...another certificate...
    },
    raw: < RAW DER buffer > ,
    valid_from: 'Nov 11 09:52:22 2009 GMT',
    valid_to: 'Nov  6 09:52:22 2029 GMT',
    fingerprint: '2A:7A:C2:DD:E5:F9:CC:53:72:35:99:7A:02:5A:71:38:52:EC:8A:DF',
    serialNumber: 'B9B0D332A1AA5635'
}
```

如果对等体不提供证书，它将返回 `null` 或一个空对象。


## tlsSocket.getCipher()

返回表示加密名称和 SSL/TLS 协议版本中定义的第一种加密方式的对象。

示例：`{ name: 'AES256-SHA', version: 'TLSv1/SSLv3' }`。

详见 [SSL_CIPHER_get_name() 和 SSL_CIPHER_get_version()](https://www.openssl.org/docs/manmaster/ssl/SSL_CIPHER_get_name.html) 了解更多信息。


## tlsSocket.getEphemeralKeyInfo()

返回一个表示在[完全正向加密](./perfect_forward_secrecy.md#)的一个客户端连接上的临时密钥交换的参数的类型、名称和大小的对象。当密钥交换不是临时的，它将返回一个空对象。由于这只支持客户端的套接字，如果在服务器端的套接字上调用，则返回 `null`。支持的类型有 `'DH'` 和 `'ECDH'`。`name` 属性只在 `'ECDH'` 中有效。

示例：

``` javascript
{ type: 'ECDH', name: 'prime256v1', size: 256 }
```


## tlsSocket.getProtocol()

返回包含当前连接协商的 SSL/TLS 协议版本的字符串。对于连接尚未完成握手过程的套接字会返回 `'unknown'`。对于服务器套接字或断开的客户端套接字会返回 `null`。

示例：

``` bash
'SSLv3'
'TLSv1'
'TLSv1.1'
'TLSv1.2'
'unknown'
```

详见 [https://www.openssl.org/docs/manmaster/ssl/SSL_get_version.html](https://www.openssl.org/docs/manmaster/ssl/SSL_get_version.html) 了解更多详情。


## tlsSocket.getSession()

返回 ASN.1 编码的会话，如果没有进行协商，则返回 `undefined`。当重新连接到服务器时，可以用于加速握手建立。


## tlsSocket.renegotiate(options, callback)

启动 TLS 重新协商过程。`options` 对象可能包含以下字段：`rejectUnauthorized`、`requestCert`。（详见 [tls.createServer()](./tls.md#tlscreateserveroptions-secureconnectionlistener) 了解更多细节。）。一旦重新成功完成协商，`callback(err)` 会用 `null` 代替 `err` 执行。

**注意**：可用于在安全连接建立后请求对等体的证书。

**另外需要注意**：当作为服务器运行时，在 `handshakeTimeout` 超时后，套接字会带着错误被销毁。


## tlsSocket.setMaxSendFragment(size)

设置最大的 TLS 片段大小（默认和最大值是：`16384`，最小值是：`512`）。当成功时，返回 `true`，否则返回 `false`。

较小的片段大小可以降低客户端上的缓冲延迟：较大的片段通过 TLS 层缓冲，直到接收到整个片段并验证其完整性；大片段可以跨越多个往返，并且由于数据包丢失或重新排序，其处理可能被延迟。然而，更小的片段会增加额外的 TLS 帧字节和 CPU 开销，这可能会降低服务器的整体吞吐量。