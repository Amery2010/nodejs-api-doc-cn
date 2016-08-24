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

* `socketPath`：Unix Domain Socket（使用 host:port 和 socketPath 的其中之一）

* `method`：一个指定 HTTP 请求方法的字符串。默认为 `'GET'`。

* `path`：请求路径。默认为 `'/'`。应包括查询字符串（如有的话）。如 `'/index.html?page=12'`。当请求的路径中包含非法字符时，会引发异常。目前，只有空字符会被拒绝，但在将来可能会发生变化。

* `headers`：一个包含请求头的对象。

* `auth`：基本身份验证，如，`'user:password'` 来计算 Authorization 头。

* `agent`：控制[代理](./class_http_Agent.md#)行为。如果使用代理请求，默认为 `Connection: keep-alive`。可能的值：

    - `undefined`（默认）：对主机和端口使用 [http.globalAgent](#httpglobalagent)。
    
    - `Agent` 对象：明确地使用代理。
    
    - `false`：选择跳出连接池的代理，默认请求 `Connection: close`。
    
* `createConnection`：当不使用 `agent` 信息选项时，产生一个用于请求的 socket/stream 的函数。这可以用于避免创建一个自定义的 Agent 类只是为了覆盖默认的 `createConnection` 函数。详见 [agent.createConnection()](./class_http_Agent.md#agentcreateconnectionoptions_callback) 了解更多信息。

可选的 `callback` 参数会被添加为 `'response'` 事件的一次性监听器。

`http.request()` 返回一个 [http.ClientRequest](./class_http_ClientRequest.md#) 类的实例。`ClientRequest` 实例是一个可写流。如果一个人想要通过 POST 请求上传一个文件，然后写入到 `ClientRequest` 对象。

示例：

``` javascript
var postData = querystring.stringify({
    'msg': 'Hello World!'
});

var options = {
    hostname: 'www.google.com',
    port: 80,
    path: '/upload',
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Content-Length': postData.length
    }
};

var req = http.request(options, (res) => {
    console.log(`STATUS: ${res.statusCode}`);
    console.log(`HEADERS: ${JSON.stringify(res.headers)}`);
    res.setEncoding('utf8');
    res.on('data', (chunk) => {
        console.log(`BODY: ${chunk}`);
    });
    res.on('end', () => {
        console.log('No more data in response.')
    })
});

req.on('error', (e) => {
    console.log(`problem with request: ${e.message}`);
});

// write data to request body
req.write(postData);
req.end();
```

请注意在示例中 `req.end()` 被调用。通过 `http.request()` 时，必须经常调用 `req.end()` 来表示你的请求已经结束 - 即使没有数据被写入请求主体。

如果请求过程中遇到任何错误（DNS 解析错误，TCP 级的错误，或实际的 HTTP 解析错误），在返回请求时，会发出 `'error'` 事件。对于所有的 `'error'` 事件而言，如果没有注册监听器，那么错误将被抛出。

这里有几个应该注意的特殊的头。

* 发送一个 `'Connection: keep-alive'` 将通知 Node.js 到服务器的连接，应一直持续到下一个请求。

* 发送一个 `'Content-length'` 头将禁用默认块编码。

* 发送一个 `'Expect'` 头会立即发送请求头。通常情况下，当发送 `'Expect: 100-continue'`，你应该设置超时并监听 `'continue'` 事件。见 RFC2616 第 8.2.3 节以获取更多信息。

* 发送 Authorization 头将覆盖使用 `auth` 选项计算基本身份验证。


## http.get(options[, callback])

由于大多数 GET 请求都没有内容，Node.js 提供了这个便捷方法。该方法与 [http.request()]() 的唯一区别是它设置该方法为 GET 并自动调用 `req.end()`。

示例：

``` javascript
http.get('http://www.google.com/index.html', (res) => {
    console.log(`Got response: ${res.statusCode}`);
    // consume response body
    res.resume();
}).on('error', (e) => {
    console.log(`Got error: ${e.message}`);
});
```