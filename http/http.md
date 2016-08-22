# 方法和属性

* [http.METHODS](#httpmethods)
* [http.STATUS_CODES](#httpstatuscodes)
* [http.globalAgent](#httpglobalagent)
* [http.createServer([requestListener])](#httpcreateserverrequestListener)
* [http.createClient([port][, host])](#httpcreateclientport_host)
* [http.request(options[, callback])](#httprequestoptions_callback)
* [http.get(options[, callback])](#httpgetoptions_callback)

--------------------------------------------------


## http.METHODS

* {Array}

由该分析器支持的 HTTP 方法列表。


## http.STATUS_CODES

* {Object}

所有标准的 HTTP 响应状态码的集合，以及各自的简短描述。例如，`http.STATUS_CODES[404] === 'Not Found'`。


## http.globalAgent

全局代理的实例，默认作为所有 HTTP 客户端的请求。


## http.createServer([requestListener])

返回一个新的 [http.Server](./class_http_Server.md#) 实例。

`requestListener` 是一个自动添加到 `'request'` 事件中的函数。


## http.createClient([port][, host])

> 稳定性：0 - 已废弃：使用 [http.request()](#httprequestoptions_callback) 代替。

构造一个新的 HTTP 客户端。`port` 和 `host` 指向被连接服务器。


## http.request(options[, callback])

Node.js 维护每台服务器的多个连接来实现 HTTP 请求。该函数允许透明地发出请求。

`options` 可以是一个对象或字符串。如果 `options` 是一个字符串，它会自动使用 [url.parse()](../url/url.md#urlparseurlstr_parsequerystring_slashesdenotehost) 解析。

选项：

* `protocol`：使用的协议。默认为 `'http:'`。

* `host`：发出请求的服务器域名或 IP 地址。默认为 `'localhost'`。

* `hostname`：`host` 的别名。以支持 [url.parse()](../url/url.md#urlparseurlstr_parsequerystring_slashesdenotehost) 解析 `hostname` 会优于 `host`。

* `family`：在解析 `hostname` 和 `host` 时所使用的 IP 地址族。有效值是 `4` 或 `6`。当它是 `unspecified` 时，将同时使用 IPv4 和 IPv6。

* `port`：远程服务器的端口。默认为 `80`。

* `localAddress`：绑定到网络连接的本地接口。

* `socketPath`：Unix Domain Socket