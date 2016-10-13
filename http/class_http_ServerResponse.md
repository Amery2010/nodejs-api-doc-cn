# http.ServerResponse类

* ['finish' 事件](#finish-事件)
* ['close' 事件](#close-事件)
* [response.statusCode](#responsestatuscode)
* [response.statusMessage](#responsestatusmessage)
* [response.headersSent](#responseheaderssent)
* [response.sendDate](#responsesenddate)
* [response.finished](#responsefinished)
* [response.setHeader(name, value)](#responsesetheadername-value)
* [response.getHeader(name)](#responsegetheadername)
* [response.removeHeader(name)](#responseremoveheadername)
* [response.addTrailers(headers)](#responseaddtrailersheaders)
* [response.writeHead(statusCode[, statusMessage][, headers])](#responsewriteheadstatuscode--statusmessage-headers)
* [response.write(chunk[, encoding][, callback])](#responsewritechunk-encoding-callback)
* [response.writeContinue()](#responsewritecontinue)
* [response.end([data][, encoding][, callback])](#responseenddata-encoding-callback)
* [response.setTimeout(msecs, callback)](#responsesettimeoutmsecs-callback)

--------------------------------------------------


## 'finish' 事件

`function () { }`

当响应已发送时发出。更具体地说，当响应报头和主体的最后一段已经被切换到操作系统用于网络传输时，发出该事件。这并不意味着该客户端已经收到任何东西。

在该事件发生后，响应对象上不再发出其他任何事件。


## 'close' 事件

`function () { }`

表示在调用 [response.end()](#responseenddata-encoding-callback) 或能够刷新之前，底层连接已终止。


## response.statusCode

当使用隐性头（没有显性地调用 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)）时，在消息头获得刷新时，此属性控制将要发送到客户端的状态码。

示例：

``` javascript
response.statusCode = 404;
```

响应头被发送到客户端后，此属性表示这是发出的状态码。


## response.statusMessage

当使用隐性头（没有显性地调用 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)）时，在消息头获得刷新时，此属性控制将要发送到客户端的状态消息。如果它的左边是 `undefined`，那么它将用于该状态码的标准报文。

示例：

``` javascript
response.statusMessage = 'Not found';
```

响应头被发送到客户端后，此属性表示这是发出的状态消息。


## response.headersSent

布尔值（只读）。如果发送了消息头则为 `true`，否则为 `false`。


## response.sendDate

如果为 `true`，如果日期头不存在于现有报头中，则它会自动生成并在响应中发送。默认为 `true`。

这应该只在测试中被禁用; HTTP 需要响应日期头。


## response.finished

布尔值，表示响应是否已完成。开始时为 `false`，在执行 [response.end()](#responseenddata-encoding-callback) 后，该值会变为 `true`。


## response.setHeader(name, value)

为隐性头设置一条单独的头内容。如果这个头内容已经存在将要发送的报头中，这个值会被覆盖。如果你想用相同的名称发送多个头内容，请使用一组字符串数组。

示例：

``` javascript
response.setHeader('Content-Type', 'text/html');
```

或

``` javascript
response.setHeader('Set-Cookie', ['type=ninja', 'language=javascript']);
```

试图设置包含无效字符的头字段名称或值会导致抛出一个 [TypeError](../errors/class_TypeError.md#)。

当消息头已经通过 [response.setHeader()](#responsesetheadername-value) 设置，它们将与任何其他的消息头合并传给 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)（优先考虑）。

``` javascript
// returns content-type = text/plain
const server = http.createServer((req, res) => {
    res.setHeader('Content-Type', 'text/html');
    res.setHeader('X-Foo', 'bar');
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('ok');
});
```


## response.getHeader(name)

读出已经排队但尚未发送到客户端的消息头。请注意，名称不区分大小写。这只能在消息头被隐性刷新前调用。

``` javascript
var contentType = response.getHeader('content-type');
```


## response.removeHeader(name)

从隐性发送队列中移除一个头内容。

示例：

``` javascript
response.removeHeader('Content-Encoding');
```


## response.addTrailers(headers)

这个方法用于给响应添加尾部头内容（一种在消息末尾的头内容）。

如果分块编码被用于响应，尾部*仅*用于发送；如果不是（如，请求是 HTTP/1.0），它们将被丢弃。

请注意，如果你打算发送尾部，HTTP 要求发送 `Trailer` 消息头，带有其值的消息头字段的列表。如，

``` javascript
response.writeHead(200, { 'Content-Type': 'text/plain',
                          'Trailer': 'Content-MD5' });
response.write(fileData);
response.addTrailers({'Content-MD5': '7895bf4b8828b55ceaf47747b4bca667'});
response.end();
```

试图设置包含无效字符的头字段名称或值会导致抛出一个 [TypeError](../errors/class_TypeError.md#)。


## response.writeHead(statusCode[, statusMessage][, headers])

给请求发送一个响应头。状态码是一个 3位数的 HTTP 状态代码，如 `404`。最后的参数 `headers`，是响应头。可选的人类可读的 `statusMessage` 是第二个参数。

示例：

``` javascript
var body = 'hello world';
response.writeHead(200, {
    'Content-Length': body.length,
    'Content-Type': 'text/plain'
});
```

该方法只能在消息中调用一次并且它必须在 [response.end()](#responseenddata-encoding-callback) 之前调用。

如果你在这之前调用 [response.write()](#responsewritechunk-encoding-callback) 或 [response.end()](#responseenddata-encoding-callback)，隐式/可变消息头将被计算并为你调用该函数。

当消息头已经通过 [response.setHeader()](#responsesetheadername-value) 设置，它们将与任何其他的消息头合并传给 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)（优先考虑）。

``` javascript
// returns content-type = text/plain
const server = http.createServer((req, res) => {
    res.setHeader('Content-Type', 'text/html');
    res.setHeader('X-Foo', 'bar');
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('ok');
});
```

请注意，`Content-Length` 是以字节为单位给出而不是字符。以上的例子可以工作时因为字符串 `'hello world'` 仅包含单字节字符。如果主体包含较高的编码的字符，那么 `Buffer.byteLength()` 应被用来确定给定内容的编码字节数。并且 Node.js 不会检查 `Content-Length` 和已发送的主体长度是否相同。

试图设置包含无效字符的头字段名称或值会导致抛出一个 [TypeError](../errors/class_TypeError.md#)。


## response.write(chunk[, encoding][, callback])

如果调用了此方法并且没有调用 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)，它会切换到隐性消息头模式并刷新隐性消息头。

它发送了一块响应主体的数据块。这种方法可以被调用多次，以便连续提供主体部分。

`chunk` 可以是字符串或一个 buffer。如果 `chunk` 是字符串，第二个参数指定如何将它编码成一个字节流。默认 `encoding` 为 `'utf8'`。当这个数据块被刷新时，最后的参数 `callback` 会被调用。

**注意**：这是原始的 HTTP 主体并且无法使用更高级别的多部分主体编码。

第一次调用 [response.write()](#responsewritechunk-encoding-callback)，它将发送缓冲的标题信息和第一主体到客户端。第二次调用 [response.write()](#responsewritechunk-encoding-callback)，Node.js 假定你要用流数据，并将它们分别发送。那便是，缓存的响应到主体的第一个数据块。

如果整个数据被成功刷新到内核缓冲区，则返回 `true`。如果数据的全部或部分在用户存储器中排队，则返回 `false`。当缓冲区再次空闲时会发出 `'drain'` 事件。


## response.writeContinue()

发送一个 HTTP/1.1 100 Continue 信息到客户端。表明该请求主体应该发送。详见 `Server` 中 ['checkContinue'](./class_http_Server.md#checkcontinue-事件) 事件。


## response.end([data][, encoding][, callback])

该方法标识着服务器端的所有响应头和主体都已经发送；该服务器应该考虑该消息完整性。对于每个响应都**必须**调用 `response.end()` 方法。

如果指定了 `data`，它等同于在 `response.end(callback)` 之后调用 [response.write(data, encoding)](#responsewritechunk-encoding-callback)。

如果指定了 `callback`，它会在响应流结束时调用。


## response.setTimeout(msecs, callback)

* `msecs` {Number}

* `callback` {Function}

设置套接字的超时时间为 `msecs`。如果提供了回调函数，它将添加作为响应对象中的 `'timeout'` 事件的监听器。

如果没有 `'timeout'` 监听器添加到请求、响应或服务器，那么套接字将在它们超时后被销毁。如果你在请求、响应或服务器的 `'timeout'` 事件上分配了处理器，那么你需要负责处理套接字的超时。

返回 `response`。