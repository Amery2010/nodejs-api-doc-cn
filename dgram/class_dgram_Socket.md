# dgram.Socket类

* ['listening' 事件](#listening-事件)
* ['message' 事件](#message-事件)
* ['close' 事件](#close-事件)
* ['error' 事件](#error-事件)
* [socket.address()](#socketaddress)
* [socket.bind(options[, callback])](#socketbindoptions-callback)
* [socket.bind([port][, address][, callback])](#socketbindport-address-callback)
* [socket.send(msg, [offset, length,] port, address[, callback])](#socketsendmsg-offset-length-port-address-callback)
* [socket.setTTL(ttl)](#socketsetttlttl)
* [socket.setMulticastTTL(ttl)](#socketsetmulticastttlttl)
* [socket.setMulticastLoopback(flag)](#socketsetmulticastloopbackflag)
* [socket.setBroadcast(flag)](#socketsetbroadcastflag)
* [socket.close([callback])](#socketclosecallback)
* [socket.addMembership(multicastAddress[, multicastInterface])](#socketaddmembershipmulticastaddress-multicastinterface)
* [socket.dropMembership(multicastAddress[, multicastInterface])](#socketdropmembership(multicastaddress-multicastinterface)
* [socket.unref()](#socketunref)
* [socket.ref()](#socketref)
* [将 socket.bind() 行为变成异步](#将-socketbind-行为变成异步)

--------------------------------------------------


`dgram.Socket` 对象是一个封装了数据报功能的 [EventEmitter](../events/class_EventEmitter.md#)。

使用 [dgram.createSocket()](./dgram.md#createsocket-options) 创建新的 `dgram.Socket` 实例。`new` 关键字不是用来创建 `dgram.Socket` 实例的。


## 'listening' 事件

每当一个套接字开始监听数据报消息时发出 `'listening'` 事件。这在创建 UDP 套接字时立即发生。


## 'message' 事件

* `msg` {Buffer} - 消息

* `rinfo` {Object} - 远程地址信息

当新的数据报在套接字上可用时，发出 `'message'` 事件。该事件处理函数传递两个参数：`msg` 和 `rinfo`。`msg` 参数是一个 [Buffer](../buffer/buffer.md#)，并且 `rinfo` 是一个具有发送方地址信息的对象，该对象提供 `address`、`family` 和 `port` 属性：

``` javascript
socket.on('message', (msg, rinfo) => {
    console.log('Received %d bytes from %s:%d\n',
        msg.length, rinfo.address, rinfo.port);
});
```


## 'close' 事件

在套接字通过 [close()](#socketclosecallback) 关闭时发出 `'close'` 事件。一旦触发，在这个套接字上将不再发出任何新的 `'message'` 事件。


## 'error' 事件

* `exception` {Error}

每当发生任何错误时，发出 `'error'` 事件。该事件处理函数传递一个单一的 Error 对象。