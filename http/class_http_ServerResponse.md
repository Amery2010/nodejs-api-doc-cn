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

当使用隐性头（没有显性地调用 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)）时，在头部获得刷新时，此属性控制将要发送到客户端的状态码。

示例：

``` javascript
response.statusCode = 404;
```

响应头被发送到客户端后，此属性表示这是发出的状态码。


## response.statusMessage

当使用隐性头（没有显性地调用 [response.writeHead()](#responsewriteheadstatuscode--statusmessage-headers)）时，在头部获得刷新时，此属性控制将要发送到客户端的状态消息。如果它的左边是 `undefined`，那么它将用于该状态码的标准报文。

示例：

``` javascript
response.statusMessage = 'Not found';
```

响应头被发送到客户端后，此属性表示这是发出的状态消息。


## response.headersSent

布尔值（只读）。如果发送了头部则为 `true`，否则为 `false`。


## response.sendDate

如果为 `true`，如果日期头不存在于现有报头中，则它会自动生成并在响应中发送。默认为 `true`。

这应该只在测试中被禁用; HTTP 需要响应日期头。


## response.finished

布尔值，表示响应是否已完成。开始时为 `false`，在执行 [response.end()](#responseenddata-encoding-callback) 后，该值会变为 `true`。