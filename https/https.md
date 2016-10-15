# 方法和属性

* [https.createServer(options[, requestListener])](#httpscreateserveroptions-requestlistener)
  - [server.listen(handle[, callback])](#serverlistenhandle-callback)
  - [server.listen(path[, callback])](#serverlistenpath-callback)
  - [server.listen(port[, host][, backlog][, callback])](#serverlistenport-host- backlog-callback)
  - [server.close([callback])](#serverclosecallback)
* [https.request(options, callback)](#httpsrequestoptions-callback)
* [https.get(options, callback)](#httpsgetoptions-callback)
* [https.globalAgent(options, callback)](#httpsglobalagentoptions-callback)

--------------------------------------------------


## https.createServer(options[, requestListener])

返回一个新的 HTTPS Web 服务器对象。`options` 类似于 [tls.createServer()](../tls/tls.md#tlscreateserveroptions-secureconnectionlistener)。`requestListener` 是一个自动添加到 `'request'` 事件上的函数。

示例：

``` javascript
// curl -k https://localhost:8000/
const https = require('https');
const fs = require('fs');

const options = {
    key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
    cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem')
};

https.createServer(options, (req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
}).listen(8000);
```

或

``` javascript
const https = require('https');
const fs = require('fs');

const options = {
    pfx: fs.readFileSync('server.pfx')
};

https.createServer(options, (req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
}).listen(8000);
```


### server.listen(handle[, callback])
### server.listen(path[, callback])
### server.listen(port[, host][, backlog][, callback])

详见 [http.listen()](../http/class_http_Server.md#)。


### server.close([callback])

详见 [http.close()](../http/class_http_Server.md#serverclosecallback)。


## https.request(options, callback)

建立一个安全的 Web 服务器的请求。

`options` 可以是一个对象或字符串。如果 `options` 是一个字符串，它会自动使用 [url.parse()](../url/url.md#urlparseurlstr_parsequerystring_slashesdenotehost) 解析。

所有的 [http.request()](../http/http.md#httprequestoptions-callback) 选项都是有效的。

示例：

``` javascript
const https = require('https');

var options = {
    hostname: 'encrypted.google.com',
    port: 443,
    path: '/',
    method: 'GET'
};

var req = https.request(options, (res) => {
    console.log('statusCode: ', res.statusCode);
    console.log('headers: ', res.headers);

    res.on('data', (d) => {
        process.stdout.write(d);
    });
});
req.end();

req.on('error', (e) => {
    console.error(e);
});
```

选项参数有以下选项：

* `host`：发出请求的服务器域名或 IP 地址。默认为 `'localhost'`。

* `hostname`：`host` 的别名。以支持 [url.parse()](../url/url.md#urlparseurlstr_parsequerystring_slashesdenotehost) 解析 `hostname` 会优于 `host`。

* `family`：在解析 `hostname` 和 `host` 时所使用的 IP 地址族。有效值是 `4` 或 `6`。当它是 `unspecified` 时，将同时使用 IPv4 和 IPv6。

* `port`：远程服务器的端口。默认为 `443`。

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
    
 