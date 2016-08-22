# HTTP(HTTP)

> 稳定度：2 - 稳定

使用 HTTP 服务器和客户端必须 `require('http')`。

在 Node.js 中的 HTTP 接口被设计为支持使用该协议中与传统不同的许多功能。尤其是，大型的可能是块编码的消息。该接口谨慎或从不缓存整个请求或响应——用户能够以流形式传输数据。

HTTP消息报头由一个像这样的对象表示：

``` javascript
{
    'content-length': '123',
    'content-type': 'text/plain',
    'connection': 'keep-alive',
    'host': 'mysite.com',
    'accept': '*/*'
}
```

键是小写，值没去修改。

为了支持全部可能的 HTTP 应用，Node.js 的 HTTP API 非常低级。它只涉及流处理和消息解析。它解析消息为头部和身体，但它并不解析实际的头部或身体。

请参阅 [message.headers](./class_http_IncomingMessage.md#messageheaders) 了解如何处理重复的头部。

收到的原始消息头被保留在 `rawHeaders` 属性中，它们是一个类似 `[key, value, key2, value2, ...]` 的数组。

例如，以前的消息头对象可能有类似以下的 `rawHeaders` 列表：

``` javascript
[
    'ConTent-Length', '123456',
    'content-LENGTH', '123',
    'content-type', 'text/plain',
    'CONNECTION', 'keep-alive',
    'Host', 'mysite.com',
    'accepT', '*/*'
]
```