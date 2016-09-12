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


该对象在内部创建并由 [http.request()](./http.md#httprequestoptions_callback)。它表示着一个*正在处理*的请求，其头部已经进入请求队列。该头部仍可以很容易地通过 `setHeader(name, value)`、`getHeader(name)` 和 `removeHeader(name)` API 进行修改。实际头部将与第一数据块或关闭连接时一起被发送。

为了获得响应对象，将一个 `'response'` 监听器添加到请求对象上。当收到响应头时，请求对象会发出一个 `'response'` 事件。`'response'` 事件只有一个执行参数，该参数是一个 [http.IncomingMessage](./class_http_IncomingMessage.md#) 实例。

在 `'response'` 事件期间，可以为响应对象添加监听器；尤其是监听 `'data'` 事件。

如果没有添加 `'response'` 处理函数，那么响应会被完全忽略。然而，如果你添加了 `'response'` 处理函数，那么你**必须**消耗从响应对象中获得的数据，每当发生 `'readable'` 事件时调用 `response.read()`，或添加一个 `'data'` 处理函数，或通过调用 `.resume()` 方法。直到数据被消耗完之前，不会发生 `'end'` 事件。同样，如果数据未被读取，它将会消耗内存，最终产生 'process out of memory' 错误。

注意：Node.js 不会检查 Content-Length 和已发送的 body 的长度是否相等。

该请求实现了 [可写流](../stream/api_for_stream_implementors.md#class_Writable) 接口。这是一个包含下列事件的 [EventEmitter](../events/class_EventEmitter.md#)：


