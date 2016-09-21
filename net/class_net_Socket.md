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
* [socket.unref()](#socketunref)
* [socket.ref()](#socketref)

--------------------------------------------------


该对象是 TCP 或本地 socket 的抽象。`net.Socket` 实例实现了一个双工（duplex）流接口。它们可以由用户创建并当成一个客户端（通过 [connect()](#socketconnectoptions-connectlistener)），或由 Node.js 创建并通过服务器的 `'connection'` 事件传递给用户。


## 'lookup' 事件

在解析主机名之后，但在连接之前发出事件。不适用于 Unix  socket 。

* `err` {Error} | {Null} 错误对象。详见 [dns.lookup()](../dns/dns.md#lookup)。

* `address` {String} IP 地址。

* `family` {String | Null} 地址类型。详见 [dns.lookup()](../dns/dns.md#lookup)。

* `host` {String} 主机名。


## 'connect' 事件

当成功建立了 socket 连接时发出。详见 [connect()](#socketconnectoptions-connectlistener)。


## 'data' 事件

* {Buffer}

当接收到数据时发出。`data` 参数会是一个 `Buffer` 或 `String`。数据的编码通过 `socket.setEncoding()` 设置（请参阅只读流章节获取更多信息）。

请注意，当一个 `Socket` 发出 `'data'` 事件时，如果没有设置监听器，**数据将会丢失**。


## 'drain' 事件

在写入 buffer 变为空时发出。可用于限制上传。

也可以看看：`socket.write()` 的返回值。


## 'end' 事件

当 socket 的另一端发送了一个 FIN 包时发生。

默认情况下（allowHalfOpen == false），一旦 socket 写完了待写队列中的数据，它就会销毁它的文件描述符。然而，通过设置 `allowHalfOpen == true`， socket 不会在它这一端自动调用 `end()`，允许用户写入任意数量的数据，用户在他们这一端要求 `end()` 会得到警告。


## 'close' 事件

* `had_error` {Boolean} `true`，如果该 socket 发生了一个传输错误。

一旦 socket 完全关闭时发出。`had_error` 参数是一个布尔值，表示 socket 是否是由于传输错误而关闭的。


## 'timeout' 事件

在 socket 闲置超时的时候发出。这只是通知 socket 已经闲置。用户必须手动关闭连接。

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


## socket.setEncoding([encoding])

设置 socket 的编码作为一个[只读流](../stream/api_for_stream_consumers.md#class_Readable)。详见 [stream.setEncoding()](../stream/api_for_stream_consumers.md#setEncoding) 了解更多信息。


## socket.setTimeout(timeout[, callback])

设置 socket 在闲置了 `timeout` 毫秒后超时。默认情况下，`net.Socket` 不会超时。当空闲超时被触发后，socket 会收到一个 ['timeout'](#timeout-事件) 事件，但连接不会被切断。用户必须手动 [end()]() 或 [destroy()]() socket。

如果 `timeout` 是 0，那么现有的空闲超时会被禁用。

可选的 `callback` 参数将被添加为 ['timeout'](#timeout-事件) 事件的一次性监听器。

返回 `socket`。


## socket.setNoDelay([noDelay])

禁用 Nagle 算法。默认情况下 TCP 连接使用 Nagle 算法，它们在发送关闭之前缓存数据。将 `noDelay` 设置为 `true`，会在每次调用 `socket.write()` 时立即发出数据。`noDelay` 默认为 `true`。

返回 `socket`。


## socket.setKeepAlive([enable][, initialDelay])

启用/禁用保持活跃功能，并且可以在空闲的 socket 发送第一个存活探测之前选择性地设置初始延迟。`enable` 默认为 `false`。

通过 `initialDelay`（以毫秒为单位）设置接收到的最后一个数据包和第一个存活探测之间的延迟。将 `initialDelay` 设置为 0，会改变默认（或之前）设置的值。默认为 `0`。

返回 `socket`。


## socket.connect(path[, connectListener])
## socket.connect(port[, host][, connectListener])

类似 [socket.connect(options[, connectListener])](#socketconnectoptions-connectlistener)，其选项为 `{port: port, host: host}` 或 `{path: path}`。


## socket.connect(options[, connectListener])

打开一个给定的 socket 连接。

对于 TCP sockets 而言，`options` 参数应该是一个特定的对象：

* `port`：应该连接到客户端端口（必需）。

* `host`：应该连接到客户端主机。默认为 `'localhost'`。

* `localAddress`：绑定到用于网络连接的本地接口。

* `localPort`：绑定到用于网络连接的本地端口。

* `family`：IP 协议栈的版本。默认为 `4`。

* `hints`：[dns.lookup()](../dns/dns.md#lookup) 提示。默认为 `0`。

* `lookup`：自定义查找功能。默认为 `dns.lookup`。

对于本地域 sockets，`options` 参数应该是一个特定的对象：

* `path`：应该连接到客户端路径（必需）。

通常不需要这种方法，作为 `net.createConnection` 打开 socket。只有当你要实现自定义的 Socket 时使用此功能。

此函数是异步的。当 ['connect'](#connect-事件) 事件发出时，会建立 socket。如果连接有问题，不会发出 `'connect'` 事件，将带着异常发出 ['error'](#data-事件) 事件。

`connectListener` 参数将被添加为 ['connect'](#connect-事件) 事件的监听器。


## socket.write(data[, encoding][, callback])

发送 socket 上的数据。第二个参数在 `data` 为字符串的情况下用于指定编码——默认为 UTF8 编码。

如果整个数据被成功刷新到内核缓冲区，则返回 `true`。如果全部或部分的数据还在用户内存队列中，则返回 `false`。当 buffer 再次清空时会发出 ['drain'](#drain-事件) 事件。

当数据最终被写入时，可选的 `callback` 参数将被执行——这可能不会立即执行。


## socket.pause()

暂停读取数据。即不会发出 ['data'](#data-事件) 事件。对于上传节流非常有用。


## socket.resume()

在调用 [pause()](#socketpause) 后恢复读取。


## socket.end([data][, encoding])

半关闭 socket。如，它发送一个 FIN 包。服务器仍然可能会发送一些数据。

如果指定了 `data`，这等同于在 `socket.end()` 后调用 `socket.write(data, encoding)`。


## socket.destroy()

确保没有更多的 I/O 活动发生此 socket 上。只有在错误（解析错误）的情况下有必要。


## socket.address()

返回绑定的地址，socket 的地址族名称和端口由操作系统所决定。返回一个具有三个属性的对象，如 `{ port: 12346, family: 'IPv4', address: '127.0.0.1' }`。


## socket.unref()

在 socket 中调用 `unref`，如果这是在事件系统中唯一活跃的 socket，将允许程序退出。如果 socket 已经 `unref`d，再次调用 `unref` 将没有效果。

返回 `socket`。


## socket.ref()

与 `unref` 相反，在一个先前 `unrefd`d socket 上调用 `ref`，如果它是唯一剩下的 socket（默认行为），将*不会*让程序退出。如果 socket 已经 `ref`d，再次调用 `ref` 将没有效果。

返回 `socket`。