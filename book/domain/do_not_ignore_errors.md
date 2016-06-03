# 警告: 不要忽视错误!

当发生错误时，域（domain）的错误处理程序不是用于代替停歇你的进程。

根据 JavaScript 中 [throw](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/throw) 的工作机制，几乎从来没有任何方式可以安全地“从你离开的地方重新拾起（pick up where you left off）”，无泄漏的引用，或创造一些其他种类的不确定脆弱状态。

对抛出的错误响应最安全的方式就是关闭进程。当然，在正常的 Web 服务器中，你可能有许多的连接打开着，因为错误是由别人引起的，就突然关闭进程，这是不合理的。

更好的方法是发送一个错误响应以触发错误的请求，而让其他人在正常的时间内完成，并停止监听该工作进程的新请求。

通过这种方式，`domain` 的用法是与集群模块携手，因为在工作进程遇到一个错误时，主进程可以产生一个新的工作进程。对于扩展到多台机器的 Node.js 程序而言，在终止代理或服务注册可以记录失败，并作出相应的响应。

例如，这不是一个好主意：

```javascript
// XXX WARNING!  BAD IDEA!

var d = require('domain').create();
d.on('error', (er) => {
    // The error won't crash the process, but what it does is worse!
    // Though we've prevented abrupt process restarting, we are leaking
    // resources like crazy if this ever happens.
    // This is no better than process.on('uncaughtException')!
    console.log('error, but oh well', er.message);
});
d.run(() => {
    require('http').createServer((req, res) => {
        handleRequest(req, res);
    }).listen(PORT);
});
```

通过使用域（domain）的上下文和弹性分离我们的程序到多个工作进程中，我们可以做到更合理地响应，并且更安全地处理错误。

```javascript
// Much better!

const cluster = require('cluster');
const PORT = +process.env.PORT || 1337;

if (cluster.isMaster) {
    // In real life, you'd probably use more than just 2 workers,
    // and perhaps not put the master and worker in the same file.
    //
    // You can also of course get a bit fancier about logging, and
    // implement whatever custom logic you need to prevent DoS
    // attacks and other bad behavior.
    //
    // See the options in the cluster documentation.
    //
    // The important thing is that the master does very little,
    // increasing our resilience to unexpected errors.

    cluster.fork();
    cluster.fork();

    cluster.on('disconnect', (worker) => {
        console.error('disconnect!');
        cluster.fork();
    });

} else {
    // the worker
    //
    // This is where we put our bugs!

    const domain = require('domain');

    // See the cluster documentation for more details about using
    // worker processes to serve requests.  How it works, caveats, etc.

    const server = require('http').createServer((req, res) => {
        var d = domain.create();
        d.on('error', (er) => {
            console.error('error', er.stack);

            // Note: we're in dangerous territory!
            // By definition, something unexpected occurred,
            // which we probably didn't want.
            // Anything can happen now!  Be very careful!

            try {
                // make sure we close down within 30 seconds
                var killtimer = setTimeout(() => {
                    process.exit(1);
                }, 30000);
                // But don't keep the process open just for that!
                killtimer.unref();

                // stop taking new requests.
                server.close();

                // Let the master know we're dead.  This will trigger a
                // 'disconnect' in the cluster master, and then it will fork
                // a new worker.
                cluster.worker.disconnect();

                // try to send an error to the request that triggered the problem
                res.statusCode = 500;
                res.setHeader('content-type', 'text/plain');
                res.end('Oops, there was a problem!\n');
            } catch (er2) {
                // oh well, not much we can do at this point.
                console.error('Error sending 500!', er2.stack);
            }
        });

        // Because req and res were created before this domain existed,
        // we need to explicitly add them.
        // See the explanation of implicit vs explicit binding below.
        d.add(req);
        d.add(res);

        // Now run the handler function in the domain.
        d.run(() => {
            handleRequest(req, res);
        });
    });
    server.listen(PORT);
}

// This part isn't important.  Just an example routing thing.
// You'd put your fancy application logic here.
function handleRequest(req, res) {
    switch (req.url) {
    case '/error':
        // We do some async stuff, and then...
        setTimeout(() => {
            // Whoops!
            flerb.bark();
        });
        break;
    default:
        res.end('ok');
    }
}
```