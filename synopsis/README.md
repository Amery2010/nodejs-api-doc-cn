# 概述

一个输出 “Hello World” 的简单 [Web 服务器](../http/) 例子：

``` javascript
const http = require('http');

http.createServer( (request, response) => {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(8124);

console.log('Server running at http://127.0.0.1:8124/');
```

要运行这个服务器，需要先将程序代码保存为 “example.js” 的文件，并使用 `node` 命令来执行：

```
$ node example.js
Server running at http://127.0.0.1:8124/
```

文档中的所有例子都可以使用相同的方式运行。