# 集群(Cluster)

> 稳定度：2 - 稳定

Node.js 的单个实例在单线程中运行。为了充分利用多核系统的用户有时想要启动 Node.js 进程的集群来处理负载。

集群模块可以使你轻松创建共享所有服务器端口的子进程。

``` javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
    // Fork workers.
    for (var i = 0; i < numCPUs; i++) {
        cluster.fork();
    }

    cluster.on('exit', (worker, code, signal) => {
        console.log(`worker ${worker.process.pid} died`);
    });
} else {
    // Workers can share any TCP connection
    // In this case it is an HTTP server
    http.createServer((req, res) => {
        res.writeHead(200);
        res.end('hello world\n');
    }).listen(8000);
}
```

运行 Node.js，现在所有的工作进程会共享 8000 端口：

``` bash
$ NODE_DEBUG=cluster node server.js
23521,Master Worker 23524 online
23521,Master Worker 23526 online
23521,Master Worker 23523 online
23521,Master Worker 23528 online
```

请注意，在 Windows 平台上，目前还不可以在一个工作进程中设置一个已命名的 pipe 服务器。