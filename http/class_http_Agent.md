# http.Agent类

* [new Agent([options])](#new_agentoptions)
* [agent.sockets](#agentsockets)
* [agent.requests](#agentrequests)
* [agent.freeSockets](#agentfreesockets)
* [agent.maxSockets](#agentmaxsockets)
* [agent.maxFreeSockets](#agentmaxfreesockets)
* [agent.createConnection(options[, callback])](#agentcreateconnectionoptions_callback)
* [agent.getName(options)](#agentgetnameoptions)
* [agent.destroy()](#agentdestroy)

--------------------------------------------------


HTTP 代理用于汇总在 HTTP 客户端请求中使用的 sockets。

HTTP 代理也是客户端请求使用 `Connection:keep-alive` 时的默认方式。如果没有待处理的 HTTP 请求，正在等待的一个 socket，会成为自由关闭的 socket。这也意味着，当在负载状态下，但仍不要求开发者使用 KeepAlive 手动关闭 HTTP 客户端时，Node.js 池有着 keep-alive 的好处。

如果你选择使用 HTTP 的 KeepAlive，你可以创建一个将标识设置为 `true` 的 Agent 对象（详见[构造器选项](#new_agentoptions)）。那么，该 Agent 将保持没用过的 sockets 都在之后使用的池中。它们将被明确标记，以便不保持 Node.js 进程运行。然而，当它们不在被使用时，明确地使用 [destroy()](agentdestroy) KeepAlive 代理仍不失为一个好方法，以便套接字被关闭。

当 socket 发出一个 `'close'` 事件或一个特殊的 `'agentRemove'` 事件时，sockets 将从代理池中移除。这意味着，如果你打算保留一个打开了很长一段时间的 HTTP 请求并且不想让它留在池中，你可以采用链式调用：

``` javascript
http.get(options, (res) => {
    // Do stuff
}).on('socket', (socket) => {
    socket.emit('agentRemove');
});
```

或者，你可以只选择完全使用 `agent:false` 退出池：

``` javascript
http.get({
    hostname: 'localhost',
    port: 80,
    path: '/',
    agent: false // create a new agent just for this one request
}, (res) => {
    // Do stuff with response
})
```


## new Agent([options])

* `options` {Object} 在代理中设置的配置选项。可以有以下字段：

  - `keepAlive` {Boolean} 保持 sockets 在池的周围以便其他的请求可以在之后使用。默认为 `false`。
  
  - `keepAliveMsecs` {Integer} 当使用 HTTP 的 KeepAlive 时，多久发送 TCP KeepAlive 报文使得 sockets 保持活跃状态。默认为 `1000`。只在 keepAlive 设置为 `true` 应用。
  
  - `maxSockets` {Number} 每个主机允许的最大 socket 数。默认为 `Infinity`。
  
  - `maxFreeSockets` {Number} 准许在自由状态下打开的最大 socket 数。只在 keepAlive 设置为 `true` 应用。默认为 `256`。
  
默认的 [http.globalAgent](./http.md#httpglobalagent) 被用于 [http.request()](./http.md#httprequestoptions_callback) 将所有这些值设置为各自的默认值。

要配置其中任何一个，你必须创建你自己的 [http.Agent](#new_agentoptions) 对象。

``` javascript
const http = require('http');
var keepAliveAgent = new http.Agent({ keepAlive: true });
options.agent = keepAliveAgent;
http.request(options, onResponseCallback);
```


## agent.sockets

其中包含当前在代理中使用的 socket 队列的对象。不要修改。


## agent.requests

其中含有尚未被分配到 sockets 的请求队列中的对象。不要修改。


## agent.freeSockets

当使用 HTTP 的 KeepAlive 时，其中包含正在等待被 Agent 使用的 socket 队列的对象。不要修改。


## agent.maxSockets

默认设置为无穷大。决定可以为每个来源打开多少个并发的 sockets 代理。来源是一个 `'host:port'` 或 `'host:port:localAddress'` 组合。


## agent.maxFreeSockets

默认设置为 256。对于代理支持 HTTP 的 KeepAlive，这是设置的在自由状态下打开的最大 socket 数。


## agent.createConnection(options[, callback])

产生一个用于 HTTP 请求的 socket/stream。

默认情况下，该函数类似于 [net.createConnection()](../net/net.md#netcreateconnectionoptions_connectListener)。然而，自定义代理可以重写此方法的情况下，期望具有更大的灵活性。

socket/stream 可以由以下两种方法提供：从该函数返回 socket/stream，或通过 `callback` 中的 socket/stream。

`callback` 有 `(err, stream)` 参数。


## agent.getName(options)

获得一个设置请求选项的唯一名称，以确定连接是否可以再利用。在 HTTP 代理中，这会返回 `host:port:localAddress`。在 HTTPS 代理中，该名称包括CA，证书，暗号和其他 HTTPS 或特定 TLS 选项来确定 socket 的可重用性。

选项：

* `host`：发出请求的服务器的域名或 IP 地址。

* `port`：远程服务器的端口。

* `localAddress`：在本地接口发出请求时绑定的网络连接。


## agent.destroy()

注销当前代理正在使用的任何 sockets。

通常没有必要做这一点。然而，如果你使用的是启用 KeepAlive 的代理，那么当你知道它不再被使用时，最好明确关闭代理。否则，在服务器终止之前 sockets 可能会被挂起开放相当长的时间。