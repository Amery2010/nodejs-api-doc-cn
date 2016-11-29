# 用法和示例

`node [options] [v8 options] [script.js | -e "script"] [arguments]`

有关使用 Node.js 运行脚本的各种选项和方法的相关信息，请参阅文档中的[命令行选项](./cli/README.md)章节。

示例：

一个使用 Node.js 编写的输出 “Hello World” 的 [Web 服务器](../http/) 示例：

``` javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World\n');
});

server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```

如果要运行这个服务器，需要先将代码保存名为 `example.js` 的文件，并使用 Node.js 来执行：

```
$ node example.js
Server running at http://127.0.0.1:3000/
```

文档中的所有示例都可以使用相同的方式运行。