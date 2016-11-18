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


## socket.address()

返回一个包含套接字地址信息的对象。对 UDP 套接字而言，这个对象会包含 `address`、`family` 和 `port` 属性。


## socket.bind(options[, callback])

* `options` {Object} - 必需。支持以下属性：

    - `port` {Number} - 必需。
    
    - `address` {String} - 可选。
    
    - `exclusive` {Boolean} - 可选。
    
* `callback` {Function} - 可选。

对于 UDP 套接字，在一个名为 `port` 和可选的 `address` 作为第一个参数传入的 `options` 对象的属性传递，使得 `dgram.Socket` 可以监听数据报消息。如果没有指定 `port`，操作系统将尝试绑定到随机端口。如果没有指定 `address`，操作系统将尝试监听所有地址。一旦完成绑定，将发出一个 `'listening'` 事件并调用可选的 `callback` 函数。

当在 [cluster](../cluster/cluster.md#) 模块中使用 `dgram.Socket` 时，`options` 对象可能包含一个额外的 `exclusive` 属性。当 `exclusive` 被设置为 `false`（默认）时，集群工作进程将使用相同的底层套接字句柄以允许共享连接来处理职责。当 `exclusive` 为 `true` 时，然而，该句柄不共享并且尝试端口共享会导致错误。

下面展示的是套接字监听专用端口的例子：

``` javascript
socket.bind({
    address: 'localhost',
    port: 8000,
    exclusive: true
});
```


## socket.bind([port][, address][, callback])

* `port` {Number} - 整数，可选。

* `address` {String} - 可选。

* `callback` {Function} - 无参数，可选。绑定完成时调用。

对于 UDP 套接字，在一个名为 `port` 和可选的 `address` 作为第一个参数传入的 `options` 对象的属性传递，使得 `dgram.Socket` 可以监听数据报消息。如果没有指定 `port`，操作系统将尝试绑定到随机端口。如果没有指定 `address`，操作系统将尝试监听所有地址。一旦完成绑定，将发出一个 `'listening'` 事件并调用可选的 `callback` 函数。

请注意，同时指定了 `'listening'` 事件监听器和传递了一个 `callback` 到 `socket.bind() ` 方法中是没有害处的，但不是很有用。

绑定的数据报套接字使得 Node.js 进程持续运行以接收数据报消息。

如果绑定失败，会生成一个 `'error'` 事件。在罕见的情况下（例如，试图绑定一个已关闭的套接字），将抛出一个 [Error](../errors/class_Error.md#)。

一个 UDP 服务器在 41234 端口上监听的例子：

``` javascript
const dgram = require('dgram');
const server = dgram.createSocket('udp4');

server.on('error', (err) => {
    console.log(`server error:\n${err.stack}`);
    server.close();
});

server.on('message', (msg, rinfo) => {
    console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`);
});

server.on('listening', () => {
    var address = server.address();
    console.log(`server listening ${address.address}:${address.port}`);
});

server.bind(41234);
// server listening 0.0.0.0:41234
```


## socket.send(msg, [offset, length,] port, address[, callback])

* `msg` {Buffer} | {String} | {Array} 要发送的消息。

* `offset` {Number} 整数。可选。在消息开始时的 buffer 中的偏移量。

* `length` {Number} 整数。可选。消息字节数。

* `port` {Number} 整数。目标端口。

* `address` {String} 目标主机名或 IP 地址。

* `callback` {Function} 当消息已发送时调用。可选。

在套接字上广播数据报。必须指定目标 `port` 和 `address`。

`msg` 参数包含要发送的消息。根据其类型，可以应用不同的行为。如果 `msg` 是一个 `Buffer`，`offset` 和 `length` 分别指定在 `Buffer` 中消息开始的偏移量和消息的字节数。如果 `msg` 是一个 `String`，那么它会自动转换为一个 `'utf8'` 编码的 `Buffer`。对于包含多字节字符的消息，`offset` 和 `length` 会计算相对[字节长度](../buffer/class_Buffer.md#Buffer_byteLength)而不是字符位置。如果 `msg` 是一个数组，不要指定 `offset` 和 `length`。

`address` 参数是一个字符串。如果 `address` 的值是一个主机名，DNS 将用于解析主机的地址。如果没有指定 `address` 或是一个空字符串，将使用 `'127.0.0.1'` 或 `'::1'`。

如果套接字之前没有给 `bind` 绑定一个调用，该套接字会被分配随机端口号，并绑定到“所有接口”地址（`'0.0.0.0'` 用于 `udp4` 套接字，`'::0'` 用于 `udp6` 套接字。）上。

可选的 `callback` 函数可以被指定为用于报告 DNS 错误或用于确定何时重用该 `buf` 对象是安全的。请注意，DNS 查找会延迟发送至少一个 Node.js 事件循环的时间。

确定数据报已发送的唯一方法是使用一个 `callback`。如果发生错误并且给出了一个 `callback`。该错误将作为第一个参数传递给 `callback`。如果没有给出 `callback`，该错误将作为 `socket` 对象上的一个 `'error'` 事件发出。

偏移量和长度是可选的，但如果你指定了其中一个，那么你必须指定另一个。此外，它们支持jin当第一个参数是一个 `Buffer` 的情况。

在 `localhost` 上，将 UDP 包发送到一个随机端口的例子：

``` javascript
const dgram = require('dgram');
const message = new Buffer('Some bytes');
const client = dgram.createSocket('udp4');
client.send(message, 41234, 'localhost', (err) => {
    client.close();
});
```

在 `localhost` 上，将由多个 buffer 组成的 UDP 包发送到一个随机端口的例子：

``` javascript
const dgram = require('dgram');
const buf1 = new Buffer('Some ');
const buf2 = new Buffer('bytes');
const client = dgram.createSocket('udp4');
client.send([buf1, buf2], 41234, 'localhost', (err) => {
    client.close();
});
```

发送多个 buffer 可能更快或更慢，具体取决于你的应用程序和操作系统：基准测试。通常它会更快。


## socket.setTTL(ttl)

* `ttl` {Number} 整数

设置 `IP_TTL` 套接字选项。虽然 TTL 通常代表“存活时间”，在此上下文中，它指定包被允许传输通过的 IP 跳的数目。转发数据包的每个路由器或网关会递减 TTL。如果 TTL 由路由器递减到 0，它将不会被转发。更改 TTL 值通常用于网络探测或组播。

`socket.setTTL()` 的参数是介于 1 和 255 之间的跳数。大多数系统的默认值是 64，但可以变化。


## socket.setMulticastTTL(ttl)

* `ttl` {Number} 整数

设置 `IP_MULTICAST_TTL` 套接字选项。虽然 TTL 通常代表“存活时间”，在此上下文中，它指定包被允许传输通过的 IP 跳的数目。转发数据包的每个路由器或网关会递减 TTL。如果 TTL 由路由器递减到 0，它将不会被转发。

传递给 `socket.setMulticastTTL()` 的参数是介于 0 和 255 之间的跳数。大多数系统的默认值是 `1`，但可以变化。


## socket.setMulticastLoopback(flag)

* `flag` {Boolean}

设置或清除 `IP_MULTICAST_LOOP` 套接字选项。当设置为 `true` 时，在本地接口上也将接收组播数据包。


## socket.setBroadcast(flag)

* `flag` {Boolean}

设置或清除 `SO_BROADCAST` 套接字选项。当设置为 `true` 时，UDP 包可以被发送到本地接口的广播地址。


## socket.close([callback])

关闭底层套接字并停止监听它上面的数据。如果提供了回调，它会被添加为 ['close'](#close-事件) 事件的一个监听器。


## socket.addMembership(multicastAddress[, multicastInterface])

* `multicastAddress` {String}

* `multicastInterface` {String} 可选。

告诉内核加入组播组，在给定的 `multicastAddress` 上使用 `IP_ADD_MEMBERSHIP` 套接字选项。如果没有提供 `multicastInterface` 参数，操作系统将尝试给所有有效的网络接口添加成员资格。


## socket.dropMembership(multicastAddress[, multicastInterface])

* `multicastAddress` {String}

* `multicastInterface` {String} 可选。

指示内核离开组播组，在 `multicastAddress` 上使用 `IP_DROP_MEMBERSHIP` 套接字选项。当套接字关闭或进程终止时，内核会自动调用此方法，因此大多数应用程序永远不会有理由去调用它。

如果没有指定 `multicastInterface`，操作系统将尝试删除所有有效接口上的成员资格。


## socket.unref()

默认情况下，绑定套接字将导致它阻止 Node.js 进程退出，只要套接字打开着。`socket.unref()` 方法可以用于从引用计数中排除套接字，从而使 Node.js 进程保持活动状态以允许进程退出，即使套接字仍在监听。

多次调用 `socket.unref()` 没有额外的效果。

`socket.unref()` 方法返回套接字的引用，因此可以链式调用。


## socket.ref()

默认情况下，绑定套接字将导致它阻止 Node.js 进程退出，只要套接字打开着。`socket.unref()` 方法可以用于从引用计数中排除套接字，从而使 Node.js 进程保持活动状态。`socket.ref()` 方法将套接字添加回引用计数并恢复默认行为。

多次调用 `socket.ref()` 没有额外的效果。

`socket.ref()` 方法返回套接字的引用，因此可以链式调用。


## 