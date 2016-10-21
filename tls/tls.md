# 方法和属性

* [tls.createServer(options[, secureConnectionListener])](#tlscreateserveroptions-secureconnectionlistener)
* [tls.connect(options[, callback])](#tlsconnectoptions-callback)
* [tls.connect(port[, host][, options][, callback])](#tlsconnectport-host-options-callback)
* [tls.createSecureContext(options)](#tlscreatesecurecontextoptions)
* [tls.createSecurePair([context][, isServer][, requestCert][, rejectUnauthorized][, options])](#tlscreatesecurepaircontext-isserver-requestcert-rejectunauthorized-options)
* [tls.getCiphers()](#tlsgetciphers)

--------------------------------------------------


## tls.createServer(options[, secureConnectionListener])

创建一个新的 [tls.Server](./class_tls_Server.md#)。`connectionListener` 参数自动设置为 ['secureConnection'](./class_tls_Server.md#secureconnection-事件) 事件的监听器。`options` 对象可能包含以下字段：

* `pfx`：一个字符串或 `Buffer`，包含服务器端 PFX 或 PKCS12 格式的私钥、证书和 CA 证书（`key` 、`cert` 和 `ca` 选项互斥）。

* `key`：一个字符串或 `Buffer`，包含服务器端 PEM 格式的私钥。为了支持多个私钥使用不同算法。它可以是一组普通的私匙或一组 `{pem: key, passphrase: passphrase}` 格式的对象。（必需）

* `passphrase`：一个包含私钥 或 pfx 口令的字符串。

* `cert`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，包含 PEM 格式的服务器证书密钥。（必需）

* `ca`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，PEM 格式的受信任的证书。如果省略，则会使用几个著名的“根”的 CA（例如 VeriSign）。这些用于授权的连接。

* `crl`：一个字符串或一组 PEM 编码的 CRLs（证书吊销列表）的字符串。

* `ciphers`：描述要使用或排除的加密字符串，使用 `:` 分隔。默认的加密套件是：

```
ECDHE-RSA-AES128-GCM-SHA256:
ECDHE-ECDSA-AES128-GCM-SHA256:
ECDHE-RSA-AES256-GCM-SHA384:
ECDHE-ECDSA-AES256-GCM-SHA384:
DHE-RSA-AES128-GCM-SHA256:
ECDHE-RSA-AES128-SHA256:
DHE-RSA-AES128-SHA256:
ECDHE-RSA-AES256-SHA384:
DHE-RSA-AES256-SHA384:
ECDHE-RSA-AES256-SHA256:
DHE-RSA-AES256-SHA256:
HIGH:
!aNULL:
!eNULL:
!EXPORT:
!DES:
!RC4:
!MD5:
!PSK:
!SRP:
!CAMELLIA
```

默认加密套件偏好 GCM 加密，[Chrome 的“现代密码学”设置](https://www.chromium.org/Home/chromium-security/education/tls#TOC-Deprecation-of-TLS-Features-Algorithms-in-Chrome)，同时也偏好 ECDHE 和 DHE 加密，完全正向加密，同时提供*一些*向后兼容性。

128 位 AES 优于 192 和 256 位 AES，[特定攻击对更大的 AES 密钥大小影响更小](https://www.schneier.com/blog/archives/2009/07/another_new_aes.html)。

老的客户端依靠不安全、不推荐使用的 RC4 或基于 DES 的加密算法（如 Internet Explorer 6）是无法完成默认配置的握手过程。如果客户端必须支持*这些*，[TLS 建议](https://wiki.mozilla.org/Security/Server_Side_TLS)可以提供一个兼容的加密套件。有关格式方面的更多细节，详见[ OpenSSL 加密列表格式文档](https://www.openssl.org/docs/apps/ciphers.html#CIPHER_LIST_FORMAT)。

* `ecdhCurve`：描述了一个名为曲线的字符串用于 ECDH 密钥协议或使用 `false` 禁用 ECDH。

默认为 `prime256v1`（NIST P-256）。使用 [crypto.getCurves()](../crypto/crypto.md#cryptogetcurves) 来获得可用的曲线名称列表。在最近的版本中，`openssl ecparam -list_curves` 也将显示名称和每个可用椭圆曲线的描述。

* `dhparam`：一个字符串或 `Buffer`，包含 Diffie Hellman 参数，需要完全正向加密。使用 `openssl dhparam` 来创建它。它的密钥长度应大于或等于 1024 位，否则它将抛出一个错误。强烈建议使用 2048 位或以上来加强安全性。如果省略或无效，它被静默丢弃并且 DHE 加密将无法使用。

* `handshakeTimeout`：如果 SSL/TLS 握手无法在指定毫秒数内完成将会中止连接。默认值是 120 秒。

每当一个握手超时，就会在 `tls.Server` 对象上发出一个 `'clientError'` 事件。

* `honorCipherOrder`：当选择一个加密时，使用服务器端偏好代替客户端偏好。默认为 `true`。

* `requestCert`：如果为 `true`，服务器将请求所连接的客户端的证书，并尝试验证证书。默认为 `false`。

* `rejectUnauthorized`：如果为 `true`，服务器将拒绝未在提供的授权 CA 列表中的任何连接。此选项仅当 `requestCert` 为 `true` 时有效。默认为 `false`。

* `NPNProtocols`：一个可能的 NPN 协议数组或 `Buffer`s。（协议应该由它们的优先级进行排序。）

* `ALPNProtocols`：一个可能的 ALPNP 协议数组或 `Buffer`s。（协议应该由它们的优先级进行排序。）当服务器从客户端同时接收 NPN 和 ALPN 扩展时，ALPN 优先于 NPN 并且服务器不会发送一个 NPN 扩展到客户端。

* `SNICallback(servername, cb)`：如果客户端支持 SNI TLS 扩展，函数将被调用。两个参数将被传递给它：`servername` 和 `cb`。`SNICallback` 应该调用 `cb(null, ctx)`，其中，ctx 一个是 `SecureContext` 实例。（`tls.createSecureContext(...)` 可以用来得到适当的 `SecureContext`）。如果未提供 `SNICallback`，将使用带有高级 API 的默认回调（见下文）。

* `sessionTimeout`：一个整数，指定在服务器创建 TLS 会话标识符之后的秒数并且服务器创建 TLS 会话凭证将超时。详见 [SSL_CTX_set_timeout](https://www.openssl.org/docs/ssl/SSL_CTX_set_timeout.html) 了解更多细节。

* `ticketKeys`：一个 48 字节的 `Buffer` 实例由一个 16 字节的前缀，16 字节的 HMAC 密钥，和一个 16 字节的 AES 密钥组成。这可以用来接受多个 TLS 服务器上的 TLS 会话凭证。

**注意**：`cluster` 模块的工作进程之间自动共享。

* `sessionIdContext`：一个字符串包含一个用于会话恢复的不透明的标识符。如果 `requestCert` 为 `true`。默认值是来自命令行产生一个 MD5 哈希值。（在 FIPS 模式中用一个截断的 SHA1 哈希代替。）否则，不提供默认值。

* `secureProtocol`：要使用的 SSL 方法，如，`SSLv3_method` 强制 SSL 版本 3。可能的值取决于你安装的 OpenSSL 并定义不变的 [SSL_METHODS](https://www.openssl.org/docs/ssl/ssl.html#DEALING_WITH_PROTOCOL_METHODS)。

下面是一个简单的响应服务器例子：

``` javascript
const tls = require('tls');
const fs = require('fs');

const options = {
    key: fs.readFileSync('server-key.pem'),
    cert: fs.readFileSync('server-cert.pem'),

    // This is necessary only if using the client certificate authentication.
    requestCert: true,

    // This is necessary only if the client uses the self-signed certificate.
    ca: [fs.readFileSync('client-cert.pem')]
};

var server = tls.createServer(options, (socket) => {
    console.log('server connected',
        socket.authorized ? 'authorized' : 'unauthorized');
    socket.write('welcome!\n');
    socket.setEncoding('utf8');
    socket.pipe(socket);
});
server.listen(8000, () => {
    console.log('server bound');
});
```

或

``` javascript
const tls = require('tls');
const fs = require('fs');

const options = {
    pfx: fs.readFileSync('server.pfx'),

    // This is necessary only if using the client certificate authentication.
    requestCert: true,

};

var server = tls.createServer(options, (socket) => {
    console.log('server connected',
        socket.authorized ? 'authorized' : 'unauthorized');
    socket.write('welcome!\n');
    socket.setEncoding('utf8');
    socket.pipe(socket);
});
server.listen(8000, () => {
    console.log('server bound');
});
```

你可以通过使用 `openssl s_client` 连接到这台服务器来做测试：

``` bash
openssl s_client -connect 127.0.0.1:8000
```


## tls.connect(options[, callback])
## tls.connect(port[, host][, options][, callback])

使用给定的 `port` 和 `host`（老 API）或 `options.port` 和 `options.host` 创建一个新的客户端连接。（如果省略 `host`，它默认为 `localhost`。）`options` 应该是一个可以指定以下属性的对象：

* `host`：客户端应该连接到主机。

* `port`：客户端应该连接到端口。

* `socket`：在一个给定的套接字上建立安全连接，而不是创建一个新的套接字。如果指定了此选项，会忽略 `host` 和 `port`。

* `path`：创建 Unix 套接字连接路径。如果指定了此选项，会忽略 `host` 和 `port`。

* `pfx`：一个字符串或 `Buffer`，包含服务器端 PFX 或 PKCS12 格式的私钥、证书和 CA 证书。

* `key`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，包含 PEM 格式的客户端私钥。

* `passphrase`：一个包含私钥 或 pfx 口令的字符串。

* `cert`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，包含 PEM 格式的客户端证书密钥。

* `ca`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，PEM 格式的受信任的证书。如果省略，则会使用几个著名的“根”的 CA（例如 VeriSign）。这些用于授权的连接。

* `ciphers`：描述要使用或排除的加密字符串，使用 `:` 分隔。默认的加密套件和 [tls.createServer()](#tlscreateserveroptions-secureconnectionlistener) 相同。

* `rejectUnauthorized`：如果为 `true`，服务器将拒绝未在提供的授权 CA 列表中的任何连接。如果验证失败，会发出一个 `'error'` 事件；`err.code` 包含 OpenSSL 错误码。默认为 `true`。

* `NPNProtocols`：一个可能的 NPN 协议字符串数组或 `Buffer`s。`Buffer`s 应该有以下格式：`0x05hello0x05world`，其中第一个字节是下一个协议的名称的长度。（传递一个数组通常会简单得多：`['hello', 'world']`）

* `ALPNProtocols`：一个可能的 ALPNP 协议字符串数组或 `Buffer`s。`Buffer`s 应该有以下格式：`0x05hello0x05world`，其中第一个字节是下一个协议的名称的长度。（传递一个数组通常会简单得多：`['hello', 'world']`）

* `servername`：用于 SNI（服务器名称指示）TLS 扩展的服务器名称。

* `checkServerIdentity(servername, cert)`：对服务器名称针对的证书提供覆盖检查。如果验证失败应该返回一个错误。如果通过，返回 `undefined`。

* `secureProtocol`：要使用的 SSL 方法，如，`SSLv3_method` 强制 SSL 版本 3。可能的值取决于你安装的 OpenSSL 并定义不变的 [SSL_METHODS](https://www.openssl.org/docs/ssl/ssl.html#DEALING_WITH_PROTOCOL_METHODS)。

* `secureContext`：从 `tls.createSecureContext( ... )` 获取的一个可选的 TLS 上下文对象。它可以被用于缓存客户端证书、密钥和 CA 证书。

* `session`：一个 `Buffer` 实例，包含 TLS 会话。

* `minDHSize`：可接受的 TLS 连接的 DH 参数的最小值（以“位”为单位）。当一台服务器提供了一个尺寸小于该值的 DH 参数，该 TLS 连接会被销毁，并抛出错误。默认：`1024`。

`callback` 参数会被添加作为 ['secureConnect'](./class_tls_Server.md#secureconnect-事件) 事件的一个监听器。

`tls.connect()` 返回一个 [tls.TLSSocket](./class_tls_TLSSocket.md#) 对象。

这里有一个之前描述的响应服务器的客户端例子：

``` javascript
const tls = require('tls');
const fs = require('fs');

const options = {
    // These are necessary only if using the client certificate authentication
    key: fs.readFileSync('client-key.pem'),
    cert: fs.readFileSync('client-cert.pem'),

    // This is necessary only if the server uses the self-signed certificate
    ca: [fs.readFileSync('server-cert.pem')]
};

var socket = tls.connect(8000, options, () => {
    console.log('client connected',
        socket.authorized ? 'authorized' : 'unauthorized');
    process.stdin.pipe(socket);
    process.stdin.resume();
});
socket.setEncoding('utf8');
socket.on('data', (data) => {
    console.log(data);
});
socket.on('end', () => {
    server.close();
});
```

或

``` javascript
const tls = require('tls');
const fs = require('fs');

const options = {
    pfx: fs.readFileSync('client.pfx')
};

var socket = tls.connect(8000, options, () => {
    console.log('client connected',
        socket.authorized ? 'authorized' : 'unauthorized');
    process.stdin.pipe(socket);
    process.stdin.resume();
});
socket.setEncoding('utf8');
socket.on('data', (data) => {
    console.log(data);
});
socket.on('end', () => {
    server.close();
});
```


## tls.createSecureContext(options)

创建一个凭据对象；该 `options` 对象可能包含以下字段：

* `pfx`：一个字符串或 `Buffer`，包含服务器端 PFX 或 PKCS12 格式的私钥、证书和 CA 证书。

* `key`：一个字符串或 `Buffer`，包含服务器端 PEM 格式的私钥。为了支持多个私钥使用不同算法。它可以是一组普通的私匙或一组 `{pem: key, passphrase: passphrase}` 格式的对象。（必需）

* `passphrase`：一个包含私钥 或 pfx 口令的字符串。

* `cert`：一个包含 PEM 编码的证书。

* `ca`：一个字符串、`Buffer`、一组字符串或一组 `Buffer`，PEM 格式的受信任的证书。如果省略，则会使用几个著名的“根”的 CA（例如 VeriSign）。这些用于授权的连接。

* `crl`：一个字符串或一组 PEM 编码的 CRLs（证书吊销列表）的字符串。

* `ciphers`：一个描述使用或排除的加密方式的字符串。在 [https://www.openssl.org/docs/apps/ciphers.html#CIPHER_LIST_FORMAT](https://www.openssl.org/docs/apps/ciphers.html#CIPHER_LIST_FORMAT) 上参考格式细节。

* `honorCipherOrder`：当选择一种加密时，请使用服务器的偏好，而不是客户端的偏好。详见 `tls` 模块文档。

如果没有给出 'CA' 细节，那么 Node.js 就会使用 CA 在 [http://mxr.mozilla.org/mozilla/source/security/nss/lib/ckfw/builtins/certdata.txt](http://mxr.mozilla.org/mozilla/source/security/nss/lib/ckfw/builtins/certdata.txt) 上给出的默认公众信任列表。


## tls.createSecurePair([context][, isServer][, requestCert][, rejectUnauthorized][, options])

创建一个新的具有两个流的安全对对象，其中一个读写经过加密数据和另一个读写明文数据。通常，已加密流通过管道输送到加密数据流，或从加密数据流输入并且明文被用作最初加密流的替代品。

* `credentials`：从 `tls.createSecureContext( ... )` 获取的一个安全的上下文对象。

* `isServer`：布尔值，指示此 TLS 连接是否应该被打开作为服务器或客户端。

* `requestCert`：布尔值，指示服务器是否应该从客户端连接请求证书。只适用于服务器的连接。

* `rejectUnauthorized`：布尔值，指示服务器是否应自动拒绝客户提供无效证书。只适用于服务器 `requestCert` 可用时。

* `options`：普通的 SSL 选项对象。详见 [tls.TLSSocket](./class_tls_TLSSocket.md#)。

`tls.createSecurePair()` 返回一个安全对对象，带有 `cleartext` 和 `encrypted` 流属性。

**注意**：`cleartext` 和 [tls.TLSSocket](./class_tls_TLSSocket.md#) 有着相同的 API。


## tls.getCiphers()

返回一个支持的 SSL 加密的名称数组。

示例：

``` javascript
var ciphers = tls.getCiphers();
console.log(ciphers); // ['AES128-SHA', 'AES256-SHA', ...]
```