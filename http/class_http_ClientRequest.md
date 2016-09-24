# http.ClientRequest类

* ['connect' 事件](#connect-事件)
* ['response' 事件](#response-事件)
* ['socket' 事件](#socket-事件)
* ['continue' 事件](#continue-事件)
* ['upgrade' 事件](#upgrade-事件)
* ['abort' 事件](#abort-事件)
* ['checkExpectation' 事件](#checkexpectation-事件)
* [request.setTimeout(timeout[, callback])](#requestsettimeouttimeout-callback)
* [request.setNoDelay([noDelay])](#requestsetnodelaynodelay)
* [request.setSocketKeepAlive([enable][, initialDelay])](#requestsetsocketkeepaliveenable-initialdelay)
* [request.flushHeaders()](#requestflushheaders)
* [request.write(chunk[, encoding][, callback])](#requestwritechunk-encoding-callback)
* [request.end([data][, encoding][, callback])](#requestenddata-encoding-callback)
* [request.abort()](#requestabort)

--------------------------------------------------


该对象在内部创建并由 [http.request()](./http.md#httprequestoptions-callback)。它表示着一个*正在处理*的请求，其头部已经进入请求队列。该头部仍可以很容易地通过 `setHeader(name, value)`、`getHeader(name)` 和 `removeHeader(name)` API 进行修改。实际头部将与第一数据块或关闭连接时一起被发送。

为了获得响应对象，将一个 `'response'` 监听器添加到请求对象上。当收到响应头时，请求对象会发出一个 `'response'` 事件。`'response'` 事件只有一个执行参数，该参数是一个 [http.IncomingMessage](./class_http_IncomingMessage.md#) 实例。

在 `'response'` 事件期间，可以为响应对象添加监听器；尤其是监听 `'data'` 事件。

如果没有添加 `'response'` 处理函数，那么响应会被完全忽略。然而，如果你添加了 `'response'` 处理函数，那么你**必须**消耗从响应对象中获得的数据，每当发生 `'readable'` 事件时调用 `response.read()`，或添加一个 `'data'` 处理函数，或通过调用 `.resume()` 方法。直到数据被消耗完之前，不会发生 `'end'` 事件。同样，如果数据未被读取，它将会消耗内存，最终产生 'process out of memory' 错误。

注意：Node.js 不会检查 Content-Length 和已发送的 body 的长度是否相等。

该请求实现了 [可写流](../stream/api_for_stream_implementors.md#class_Writable) 接口。这是一个包含下列事件的 [EventEmitter](../events/class_EventEmitter.md#)：


## 'connect' 事件

`function (response, socket, head) { }`

每当服务器响应一个带有 `CONNECT` 方法的请求时都会发生。如果没有监听该事件，客户端接收到 `CONNECT` 方法后会将有自己的连接关闭。

一对客户端和服务端组合会向你展示如何监听 `'connect'` 事件。

``` javascript
const http = require('http');
const net = require('net');
const url = require('url');

// Create an HTTP tunneling proxy
var proxy = http.createServer((req, res) => {
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('okay');
});
proxy.on('connect', (req, cltSocket, head) => {
    // connect to an origin server
    var srvUrl = url.parse(`http://${req.url}`);
    var srvSocket = net.connect(srvUrl.port, srvUrl.hostname, () => {
        cltSocket.write('HTTP/1.1 200 Connection Established\r\n' +
            'Proxy-agent: Node.js-Proxy\r\n' +
            '\r\n');
        srvSocket.write(head);
        srvSocket.pipe(cltSocket);
        cltSocket.pipe(srvSocket);
    });
});

// now that proxy is running
proxy.listen(1337, '127.0.0.1', () => {

    // make a request to a tunneling proxy
    var options = {
        port: 1337,
        hostname: '127.0.0.1',
        method: 'CONNECT',
        path: 'www.google.com:80'
    };

    var req = http.request(options);
    req.end();

    req.on('connect', (res, socket, head) => {
        console.log('got connected!');

        // make a request over an HTTP tunnel
        socket.write('GET / HTTP/1.1\r\n' +
            'Host: www.google.com:80\r\n' +
            'Connection: close\r\n' +
            '\r\n');
        socket.on('data', (chunk) => {
            console.log(chunk.toString());
        });
        socket.on('end', () => {
            proxy.close();
        });
    });
});
```


## 'response' 事件

`function (response) { }`

当收到该请求的响应时发生。该事件只发生一次。`response` 的参数会是一个 [http.IncomingMessage](./class_http_IncomingMessage.md#) 的实例。


## 'socket' 事件

`function (socket) { }`

在该请求分配到一个 socket 后发生。


## 'continue' 事件

`function () { }`

当服务器发送了一个 '100 Continue' 的 HTTP 响应时发生，通常因为该请求包含 'Expect: 100-continue'。这是客户端应发送请求主体的指令。


## 'upgrade' 事件

`function (response, socket, head) { }`

每当服务器响应一个升级请求时发生。如果没有监听该事件，客户端接收到一个升级头部后会将有自己的连接关闭。

一对客户端和服务端组合会向你展示如何监听 `'upgrade'` 事件。

``` javascript
const http = require('http');

// Create an HTTP server
var srv = http.createServer((req, res) => {
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('okay');
});
srv.on('upgrade', (req, socket, head) => {
    socket.write('HTTP/1.1 101 Web Socket Protocol Handshake\r\n' +
        'Upgrade: WebSocket\r\n' +
        'Connection: Upgrade\r\n' +
        '\r\n');

    socket.pipe(socket); // echo back
});

// now that server is running
srv.listen(1337, '127.0.0.1', () => {

    // make a request
    var options = {
        port: 1337,
        hostname: '127.0.0.1',
        headers: {
            'Connection': 'Upgrade',
            'Upgrade': 'websocket'
        }
    };

    var req = http.request(options);
    req.end();

    req.on('upgrade', (res, socket, upgradeHead) => {
        console.log('got upgraded!');
        socket.end();
        process.exit(0);
    });
});
```


## 'abort' 事件

`function () { }`

当请求已经被客户端中止时发出。该事件只发生在第一次调用 `abort()`。


## 'checkExpectation' 事件

`function (request, response) { }`

每当收到一个带有 http Expect ，但值不是 '100-continue' 的请求头部时发生。如果没有监听该事件，服务器会自动响应一个适当的 `417 Expectation Failed`。

注意，当事件发生和处理后，`request` 事件将不会发生。


## request.setTimeout(timeout[, callback])

一旦 socket 被分配给该请求，关联的 [socket.setTimeout()](../net/class_net_Socket.md#socketsettimeouttimeout-callback) 会被调用。

* `timeout` {Number} 请求被认为是超时的毫秒数。

* `callback` {Function} 可选的，当发生超时时调用的函数。同样绑定到 `timeout` 事件。


## request.setNoDelay([noDelay])

一旦 socket 被分配给该请求，关联的 [socket.setNoDelay()](../net/class_net_Socket.md#socketsetnodelaynodelay) 会被调用。


## request.setSocketKeepAlive([enable][, initialDelay])

一旦 socket 被分配给该请求，关联的 [socket.setKeepAlive()](../net/class_net_Socket.md#socketsetkeepaliveenable-initialdelay) 会被调用。


## request.flushHeaders()

强制刷新请求头。

出于效率的考虑，Node.js 通常缓存请求头直到你调用 `request.end()` 或写入请求数据的第一个数据块。然后，它试图将请求头和数据封装成单一的 TCP 数据包。

这通常是你想要的（它节省了 TCP 往返），除了当第一个数据块直到很久之后才会被发送的情况。`request.flushHeaders()` 让你绕过优化并提前开始请求。


## request.write(chunk[, encoding][, callback])

发送一个主体数据块。通过多次调用该方法，用户可以以流的形式将请求主体发送到服务器 —— 在这种情况下，当创建请求时，建议使用 `['Transfer-Encoding', 'chunked']` 头部行。

该 `chunk` 参数应该是一个 [Buffer](../buffer/buffer.md#) 或字符串。

该 `encoding` 参数是可选的，并仅适用于 `chunk` 是字符串的情况。默认为 `'utf8'`。

该 `callback` 参数是可选的，并在数据块刷新时调用。

返回 `request`。


## request.end([data][, encoding][, callback])

完成发送请求。如果主体中的任何部分未被发送，它会将它们刷新流中。如果请求被分块，它将发送终止 `'0\r\n\r\n'`。

如果指定了 `data`，这等同于在 `request.end(callback)` 之后调用 [response.write(data, encoding)](./class_http_ServerResponse.md#responsewritechunk-encoding-callback)。

如果指定了 `callback`，当请求流结束时，它会被调用。


## request.abort()

标志着请求终止。调用该方法将导致剩余的响应数据被丢弃，并销毁 socket。