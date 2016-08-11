# net.Socket类

* ['lookup' 事件](#lookup-事件)
* ['connect' 事件](#connect-事件)
* ['data' 事件](#data-事件)
* ['drain' 事件](#drain-事件)
* ['end' 事件](#end-事件)
* ['close' 事件](#close-事件)
* ['timeout' 事件](#timeout-事件)
* ['error' 事件](#error-事件)
* [new net.Socket([options])](#new_netsocketoptions)
* [socket.bufferSize](#socketbuffersize)
* [socket.bytesRead](#socketbytesread)
* [socket.bytesWritten](#socketbyteswritten)
* [socket.localPort](#socketlocalPort)
* [socket.localAddress](#socketlocaladdress)
* [socket.remotePort](#socketremoteport)
* [socket.remoteFamily](#socketremotefamily)
* [socket.remoteAddress](#socketremoteaddress)
* [socket.setEncoding([encoding])](#socketsetencodingencoding)
* [socket.setTimeout(timeout[, callback])](#socketsettimeouttimeout-callback)
* [socket.setNoDelay([noDelay])](#socketsetnodelaynodelay)
* [socket.setKeepAlive([enable][, initialDelay])](#socketsetkeepaliveenable-initialdelay)
* [socket.connect(path[, connectListener])](#socketconnectpath-connectlistener)
* [socket.connect(port[, host][, connectListener])](#socketconnectport-host-connectlistener)
* [socket.connect(options[, connectListener])](#socketconnectoptions-connectlistener)
* [socket.write(data[, encoding][, callback])](#socketwritedata-encoding-callback)
* [socket.pause()](#socketpause)
* [socket.resume()](#socketresume)
* [socket.end([data][, encoding])](#socketenddata-encoding)
* [socket.destroy()](#socketdestroy)
* [socket.address()](#socketaddress)
* [socket.ref()](#socketref)
* [socket.unref()](#socketunref)

--------------------------------------------------


该对象是 TCP 或本地套接字的抽象。`net.Socket` 实例实现了一个双工（duplex）流接口。它们可以由用户创建并当成一个客户端（通过 [connect()](#socketconnectoptions-connectlistener)），或由 Node.js 创建并通过服务器的 `'connection'` 事件传递给用户。


## 'lookup' 事件

在解析主机名之后，但在连接之前发出事件。不适用于 Unix 套接字。

* `err` {Error} | {Null} 错误对象。详见 [dns.lookup()](../dns/dns.md#lookup)。

* `address` {String} IP 地址。

* `family` {String | Null} 地址类型。详见 [dns.lookup()](../dns/dns.md#lookup)。

* `host` {String} 主机名。


## 'connect' 事件

当成功建立了套接字连接时发出。详见 [connect()](#socketconnectoptions-connectlistener)。


## 'data' 事件

* {Buffer}

当接收到数据时发出。`data` 参数会是一个 `Buffer` 或 `String`。数据的编码通过 `socket.setEncoding()` 设置（请参阅只读流章节获取更多信息）。

请注意，当一个 `Socket` 发出 `'data'` 事件时，如果没有设置监听器，**数据将会丢失**。


## 'drain' 事件

在写入 buffer 变为空时发出。可用于限制上传。

也可以看看：`socket.write()` 的返回值。


## 'end' 事件

当套接字的另一端发送了一个 FIN 包时发生。

默认情况下（allowHalfOpen == false），一旦套接字写完了待写队列中的数据，它就会销毁它的文件描述符。然而，通过设置 `allowHalfOpen == true`，套接字不会在它这一端自动调用 `end()`，允许用户写入任意数量的数据，用户在他们这一端要求 `end()` 会得到警告。


## 'close' 事件

* `had_error` {Boolean} `true`，如果该套接字发生了一个传输错误。

一旦套接字完全关闭时发出。`had_error` 参数是一个布尔值，表示套接字是否是由于传输错误而关闭的。


## 'timeout' 事件

在套接字闲置超时的时候发出。这只是通知套接字已经闲置。用户必须手动关闭连接。

也可以看看：[socket.setTimeout()](socketsettimeouttimeout-callback)。


## 'error' 事件

* {Error}

在发生错误时发出。`'close'` 事件会直接在该事件之后调用。