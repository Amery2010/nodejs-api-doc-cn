# 显式绑定

有时，正在使用的域（domain）不是应该被用于一个特定的事件触发器的其中之一。或者，该事件触发器可能已在一个域的上下文中创建，但应该代替绑定到一些其他的域上。

例如，可能有一个正在被 HTTP 服务器使用的域，但也许我们想为每个请求使用一个单独的域。

这可以通过显式绑定实现。

例如：

```javascript
// create a top-level domain for the server
const domain = require('domain');
const http = require('http');
const serverDomain = domain.create();

serverDomain.run(() => {
    // server is created in the scope of serverDomain
    http.createServer((req, res) => {
        // req and res are also created in the scope of serverDomain
        // however, we'd prefer to have a separate domain for each request.
        // create it first thing, and add req and res to it.
        var reqd = domain.create();
        reqd.add(req);
        reqd.add(res);
        reqd.on('error', (er) => {
            console.error('Error', er, req.url);
            try {
                res.writeHead(500);
                res.end('Error occurred, sorry.');
            } catch (er) {
                console.error('Error sending 500', er, req.url);
            }
        });
    }).listen(1337);
});
```