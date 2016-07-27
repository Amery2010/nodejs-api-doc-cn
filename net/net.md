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


<div id="delimiter" class="anchor"></div>
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