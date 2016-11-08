# 方法和属性

* [dgram.createSocket(options[, callback])](#createsocket-options)
* [dgram.createSocket(type[, callback])](#createsocket-type)

--------------------------------------------------

## dgram.createSocket(options[, callback])

* `options` {Object}

* `callback` {Function} 作为 `'message'` 事件的一个附属监听器。

* 返回：[dgram.Socket>](./class_dgram_Socket.md#)

创建一个 `dgram.Socket` 对象。`options` 参数是一个应该包含一个 `udp4` 或 `udp6` 的 `type` 字段和一个可选的 `reuseAddr` 布尔型字段的对象。

当 `reuseAddr` 为 `true` 时，[socket.bind()](./class_dgram_Socket.md#socketbindport-address-callback) 将重用该地址，即使另一个进程已经绑定了一个套接字。`reuseAddr` 默认为 `false`。一个可选的 `callback` 函数可以通过指定添加为 `'message'` 事件的一个监听器。

一旦创建了套接字，调用 [socket.bind()](./class_dgram_Socket.md#socketbindport-address-callback) 会指示套接字开始监听数据报消息。当 `address` 和 `port` 没有传给 [socket.bind()](./class_dgram_Socket.md#socketbindport-address-callback) 时，该方法将会在一个随机端口上绑定套接字到“所有接口”地址（它对 `udp4` 和 `udp6` 套接字都是正确的）。绑定的地址和端口可以使用 [socket.address().address](./class_dgram_Socket.md#socket.address) 和 [socket.address().port](./class_dgram_Socket.md#socket.address)  检索。


## dgram.createSocket(type[, callback])

* `type` {String} 既可以是 `'udp4'` 也可以是 `'udp6'`

* `callback` {Function} 作为 `'message'` 事件的一个附属监听器。可选

* 返回：[dgram.Socket>](./class_dgram_Socket.md#)

通过指定的 `type` 创建一个 `dgram.Socket` 对象。`type` 参数既可以是 `udp4`，也可以是 `udp6`。一个可选的 `callback` 函数可以通过指定添加为 `'message'` 事件的一个监听器。

一旦创建了套接字，调用 [socket.bind()](./class_dgram_Socket.md#socketbindport-address-callback) 会指示套接字开始监听数据报消息。当 `address` 和 `port` 没有传给 [socket.bind()](./class_dgram_Socket.md#socketbindport-address-callback) 时，该方法将会在一个随机端口上绑定套接字到“所有接口”地址（它对 `udp4` 和 `udp6` 套接字都是正确的）。绑定的地址和端口可以使用 [socket.address().address](./class_dgram_Socket.md#socket.address) 和 [socket.address().port](./class_dgram_Socket.md#socket.address)  检索。