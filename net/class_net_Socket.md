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


## new net.Socket([options])

构造一个新的 Socket 对象。

`options` 是一个带有以下默认值的对象：

``` javascript
{
    fd: null,
    allowHalfOpen: false,
    readable: false,
    writable: false
}
```

`fd` 允许你指定现有的 socket 文件描述符。设置 `readable` 或 `writable` 为 `true` 将允许在该 socket 上读取或写入（注意：仅当传入 `fd` 时有效）。关于 `allowHalfOpen`，请参考 `createServer()` 和 `'end'` 事件。

`net.Socket` 实例是 [EventEmitter](../events/class_EventEmitter.md#)。


## socket.bufferSize

`net.Socket` 有该属性以保证 `socket.write()` 总是有效。这是为了帮助用户快速启动和运行。计算机不能总是赶上写入到一个 socket 中的数据量——网络连接只可能太慢。Node.js 在内部以队列形式写入到 socket 中，并在合适时机通过报文发送。（在内部，Node.js 轮询 socket 的文件描述符以保证可写）

内部缓存可能会导致内存占用增加。该属性表示要被写入到当前缓冲的字符数。（字符数大约等于被写入的字节数，但 buffer 可能包含字符串并且该字符串是惰性编码的，所以无法知晓字节的确切数目。）

遇到大的或增长的 `bufferSize` 的用户，应该在他们的程序中尝试通过 [pause()](#socketpause) 和 [resume()](#socketresume) 进行数据“节流”。


## socket.bytesRead

已接收的字节数。


## socket.bytesWritten

发送的字节数。


## socket.localPort

该数字表示本地端口。例如，`80` 或 `21`。


## socket.localAddress

该字符串表示用于远程客户端连接的本地 IP 地址。例如，如果你监听 `'0.0.0.0'` 并且客户端连接到了 `'192.168.1.1'`，该值将会是 `'192.168.1.1'`。


## socket.remotePort

该数字表示远程端口。例如，`80` 或 `21`。


## socket.remoteFamily

该数字表示远程 IP 族。`'IPv4'` 或 `'IPv6'`。


## socket.remoteAddress

该数字表示远程 IP 地址。例如，`'74.125.127.100'` 或 `'2001:4860:a005::68'`。如果 socket 被销毁，那么值也可能是 `undefined`（例如，假设客户端断开连接）。