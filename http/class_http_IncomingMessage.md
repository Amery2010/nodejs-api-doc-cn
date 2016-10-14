# http.IncomingMessage类

* ['close' 事件](#close-事件)
* [message.httpVersion](#messagehttpversion)
* [message.url](#messageurl)
* [message.socket](#messagesocket)
* [message.method](#messagemethod)
* [message.headers](#messageheaders)
* [message.trailers](#messagetrailers)
* [message.rawHeaders](#messagerawheaders)
* [message.rawTrailers](#messagerawtrailers)
* [message.statusCode](#messagestatuscode)
* [message.statusMessage](#messagestatusmessage)
* [message.setTimeout(msecs, callback)](#messagesettimeoutmsecs-callback)

--------------------------------------------------


`IncomingMessage` 对象由 [http.Server](./class_http_Server.md#) 或 [http.ClientRequest](./class_http_ClientRequest.md#) 创建，并作为第一参数分别递给 `'request'` 和 `'response'` 事件。它可以用来访问响应状态，报头和数据。

它实现了[可读流](../stream/api_for_stream_implementors.md#)接口，以及下面的其他事件、方法和属性。


## 'close' 事件

`function () { }`

表明基础连接已关闭。跟 `'end'` 一样，这个事件对于每个响应只会触发一次。


## message.httpVersion

在服务器请求的情况下，HTTP 版本号由客户端发送。在客户端响应的情况下，HTTP 版本由所连接到服务器决定。也许是 `'1.1'` 或 `'1.0'`。

同样，`message.httpVersionMajor` 是第一个整数，`message.httpVersionMinor` 是第二个整数。


## message.url

**仅适用于从 [http.Server](./class_http_Server.md#) 获得的请求。**

请求的 URL 字符串。这仅包含实际存在的 HTTP 请求的 URL。如果请求是：

``` javascript
GET /status?name=ryan HTTP/1.1\r\n
Accept: text/plain\r\n
\r\n
```

那么 `request.url` 会是：

``` javascript
'/status?name=ryan'
```

如果你想将 URL 解析成其组成部分，你可以使用 `require('url').parse(request.url)`。例如：

``` bash
$ node
> require('url').parse('/status?name=ryan')
{
    href: '/status?name=ryan',
    search: '?name=ryan',
    query: 'name=ryan',
    pathname: '/status'
}
```

如果你想从查询字符串中提取参数，你可以使用 `require('querystring').parse` 函数，或传递 `true` 作为 `require('url').parse` 的第二个参数。例如：

``` bash
$ node
> require('url').parse('/status?name=ryan', true)
{
    href: '/status?name=ryan',
    search: '?name=ryan',
    query: {
        name: 'ryan'
    },
    pathname: '/status'
}
```


## message.socket

与此连接相关联的 [net.Socket](../net/class_net_Socket.md#) 对象。

通过 HTTPS 的支持，使用 [request.socket.getPeerCertificate()](../tls/class_tls_TLSSocket.md#tlssocketgetpeercertificatedetailed) 获取客户端的认证信息。


## message.method

**仅适用于从 [http.Server](./class_http_Server.md#) 获得的请求。**

请求方法为字符串。只读。例如：`'GET'`，`'DELETE'`。


## message.header

请求/响应头的对象。

键值对的报头名称和值。报头名称为小写。例如：

``` javascript
// Prints something like:
//
// { 'user-agent': 'curl/7.22.0',
//   host: '127.0.0.1:8000',
//   accept: '*/*' }
console.log(request.headers);
```

原报头按以下方式根据不同的头名进行重复处理：

* 重复 `age`，`authorization`，`content-length`，`content-type`，`etag`，`expires`，`from`，`host`，`if-modified-since`，`if-unmodified-since`，`last-modified`，`location`，`max-forwards`，`proxy-authorization`，`referer`，`retry-after` 或已丢弃的 `user-agent`。

* `set-cookie` 始终是一个数组。重复被添加到队列。

* 对于所有其他头，其值使用“,”相连。


## message.trailers

请求/响应报尾对象。只在 `'end'` 事件时填入。


## message.rawHeaders

接收到的原始请求/响应头字段列表。

注意，该键和值是在同一列表中。它*不是*一个元组列表。偶数偏移量为键，奇数偏移量为对应的值。

报头名称没有转换为小写，也没有合并重复的头。

``` javascript
// Prints something like:
//
// [ 'user-agent',
//   'this is invalid because there can be only one',
//   'User-Agent',
//   'curl/7.22.0',
//   'Host',
//   '127.0.0.1:8000',
//   'ACCEPT',
//   '*/*' ]
console.log(request.rawHeaders);
```


## message.rawTrailers

接收到的原始的请求/响应报尾的键和值。只在 `'end'` 事件时填入。


## message.statusCode

**仅适用于从 [http.ClientRequest](./class_http_ClientRequest.md#) 获得响应。**

3位 HTTP 响应状态码。如，`404`。


## message.statusMessage

**仅适用于从 [http.ClientRequest](./class_http_ClientRequest.md#) 获得响应。**

HTTP 响应状态消息（简短的原因）。如，`OK` 或 `Internal Server Error`。


## message.setTimeout(msecs, callback)

* `msecs` {Number}

* `callback` {Function}

调用 `message.connection.setTimeout(msecs, callback)`。

返回 `message`。