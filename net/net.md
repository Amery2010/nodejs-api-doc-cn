# 方法和属性

* [net.createServer([options][, connectionListener])](#createServer)
* [net.createConnection(options[, connectListener])](#createConnection_options)
* [net.createConnection(path[, connectListener])](#createConnection_path)
* [net.createConnection(port[, host][, connectListener])](#createConnection_port)
* [net.connect(options[, connectListener])](#connect_options)
* [net.connect(path[, connectListener])](#connect_path)
* [net.connect(port[, host][, connectListener])](#connect_port)
* [net.isIP(input)](#isIP)
* [net.isIPv4(input)](#isIPv4)
* [net.isIPv6(input)](#isIPv6)

--------------------------------------------------


<div id="createServer" class="anchor"></div>
## net.createServer([options][, connectionListener])

创建一个新的服务器。`connectionListener` 参数自动设置为 ['connection' 事件](./class_net_Server.md#event_connection) 的一个监听器。

`options` 是一个对象包含以下默认值：

``` javascript
{
    allowHalfOpen: false,
    pauseOnConnect: false
}
```

如果 `allowHalfOpen` 是 `true`，那么当其他 socket 端发送了一个 FIN 包时，socket 不会自动发送 FIN 包。该 socket 变成不可读但可写的状态。你应该明确地调用 [end()](./class_net_Socket.md#end) 方法。详见 ['end'](./class_net_Socket.md#event_end) 事件了解更多详情。

如果 `pauseOnConnect` 是 `true`，那么与每个传入连接关联的 socket 都会被暂停，并且从它的句柄中无法读取数据。这使得连接在不读取原始进程的数据的情况下，在进程之间传递。调用 [resume()](./class_net_Socket.md#resume) 从暂停 socket 中开始读取数据。

这是一个监听端口 8124 的连接的回声服务器例子：

``` javascript
const net = require('net');
const server = net.createServer((c) => {
    // 'connection' listener
    console.log('client connected');
    c.on('end', () => {
        console.log('client disconnected');
    });
    c.write('hello\r\n');
    c.pipe(c);
});
server.on('error', (err) => {
    throw err;
});
server.listen(8124, () => {
    console.log('server bound');
});
```

使用 `telnet` 进行测试：

``` bash
telnet localhost 8124
```

要监听 socket `/tmp/echo.sock`，只要将倒数的第三行改为

``` javascript
server.listen('/tmp/echo.sock', () => {
    console.log('server bound');
});
```

使用 `nc` 来连接一个 `UNIX` 域 socket 服务器：

``` javascript
nc -U /tmp/echo.sock
```


<div id="createConnection_options" class="anchor"></div>
## net.createConnection(options[, connectListener])

一个工厂函数，返回一个新的 [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `options` 自动连接。

`options` 同时被传到 [net.Socket](./class_net_Socket.md#) 构造函数和 [socket.connect](./class_net_Socket.md#connect) 方法。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。

这是先前描述的回声服务器的客户端的一个例子：

``` javascript
const net = require('net');
const client = net.createConnection({
    port: 8124
}, () => {
    //'connect' listener
    console.log('connected to server!');
    client.write('world!\r\n');
});
client.on('data', (data) => {
    console.log(data.toString());
    client.end();
});
client.on('end', () => {
    console.log('disconnected from server');
});
```

要监听 socket `/tmp/echo.sock`，只要将第二行改为

``` javascript
const client = net.createConnection({path: '/tmp/echo.sock'});
```


<div id="createConnection_path" class="anchor"></div>
## net.createConnection(path[, connectListener])

一个工厂函数，返回一个新的 unix [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `path` 自动连接。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。


<div id="createConnection_port" class="anchor"></div>
## net.createConnection(port[, host][, connectListener])

一个工厂函数，返回一个新的 [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `path` 和 `host` 自动连接。

如果省略了 `host`，`'localhost'` 将会承担其职责。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。


<div id="connect_options" class="anchor"></div>
## net.connect(options[, connectListener])

一个工厂函数，返回一个新的 [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `options` 自动连接。

`options` 同时被传到 [net.Socket](./class_net_Socket.md#) 构造函数和 [socket.connect](./class_net_Socket.md#connect) 方法。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。

这是先前描述的回声服务器的客户端的一个例子：

``` javascript
const net = require('net');
const client = net.connect({
    port: 8124
}, () => {
    // 'connect' listener
    console.log('connected to server!');
    client.write('world!\r\n');
});
client.on('data', (data) => {
    console.log(data.toString());
    client.end();
});
client.on('end', () => {
    console.log('disconnected from server');
});
```

要监听 socket `/tmp/echo.sock`，只要将第二行改为

``` javascript
const client = net.connect({path: '/tmp/echo.sock'});
```


<div id="connect_path" class="anchor"></div>
## net.connect(path[, connectListener])

一个工厂函数，返回一个新的 unix [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `path` 自动连接。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。


<div id="connect_port" class="anchor"></div>
## net.connect(port[, host][, connectListener])

一个工厂函数，返回一个新的 [net.Socket](./class_net_Socket.md#) 实例，并使用提供的 `path` 和 `host` 自动连接。

如果省略了 `host`，`'localhost'` 将会承担其职责。

`connectListener` 参数会被添加为 ['connect' 事件](./class_net_Socket.md#event_connect) 的监听器一次。


<div id="isIP" class="anchor"></div>
## net.isIP(input)

检测是否输入是一个 IP 地址。对于无效的字符串，返回 0，对于 IPv4 的地址返回 4，对于 IPv6 地址返回 6。


<div id="isIPv4" class="anchor"></div>
## net.isIPv4(input)

如果输入的是版本 4 的 IP 地址，则返回 true，否则返回 false。


<div id="isIPv6" class="anchor"></div>
## net.isIPv6(input)

如果输入的是版本 6 的 IP 地址，则返回 true，否则返回 false。