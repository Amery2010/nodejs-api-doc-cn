# 示例

压缩或解压缩一个文件可以通过导流一个 fs.ReadStream 到一个 zlib 流，然后到一个 fs.WriteStream 来完成。

```javascript
const gzip = zlib.createGzip();
const fs = require('fs');
const inp = fs.createReadStream('input.txt');
const out = fs.createWriteStream('input.txt.gz');

inp.pipe(gzip).pipe(out);
```

一步压缩或解压缩数据可以通过快捷方法来完成。

```javascript
const input = '.................................';
zlib.deflate(input, (err, buffer) => {
    if (!err) {
        console.log(buffer.toString('base64'));
    } else {
        // handle error
    }
});

const buffer = new Buffer('eJzT0yMAAGTvBe8=', 'base64');
zlib.unzip(buffer, (err, buffer) => {
    if (!err) {
        console.log(buffer.toString());
    } else {
        // handle error
    }
});
```

要在 HTTP 客户端或服务器中使用此模块，请在请求中使用 [accept-encoding](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3) 和在响应中使用 [content-encoding](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.11) 头。

**注意：这些例子只是极其简单地展示了基础的概念**。Zlib 编码消耗非常大，其结果应当被缓存。详见 [优化内存占用]() 中更多的关于 Zlib 用法中对速度 / 内存 / 压缩的权衡取舍。

```javascript
// client request example
const zlib = require('zlib');
const http = require('http');
const fs = require('fs');
const request = http.get({
    host: 'izs.me',
    path: '/',
    port: 80,
    headers: {
        'accept-encoding': 'gzip,deflate'
    }
});
request.on('response', (response) => {
    var output = fs.createWriteStream('izs.me_index.html');

    switch (response.headers['content-encoding']) {
        // or, just use zlib.createUnzip() to handle both cases
    case 'gzip':
        response.pipe(zlib.createGunzip()).pipe(output);
        break;
    case 'deflate':
        response.pipe(zlib.createInflate()).pipe(output);
        break;
    default:
        response.pipe(output);
        break;
    }
});

// server example
// Running a gzip operation on every request is quite expensive.
// It would be much more efficient to cache the compressed buffer.
const zlib = require('zlib');
const http = require('http');
const fs = require('fs');
http.createServer((request, response) => {
    var raw = fs.createReadStream('index.html');
    var acceptEncoding = request.headers['accept-encoding'];
    if (!acceptEncoding) {
        acceptEncoding = '';
    }

    // Note: this is not a conformant accept-encoding parser.
    // See http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3
    if (acceptEncoding.match(/\bdeflate\b/)) {
        response.writeHead(200, {
            'content-encoding': 'deflate'
        });
        raw.pipe(zlib.createDeflate()).pipe(response);
    } else if (acceptEncoding.match(/\bgzip\b/)) {
        response.writeHead(200, {
            'content-encoding': 'gzip'
        });
        raw.pipe(zlib.createGzip()).pipe(response);
    } else {
        response.writeHead(200, {});
        raw.pipe(response);
    }
}).listen(1337);
```

默认情况下，Zlib 方法在截断解压缩数据时，会抛出一个错误。然而，如果已知该数据是不完整的，或仅仅是希望检查一个压缩文件的开始部分，通过改变用于压缩输入数据最后一个数据块的 flushing 方法，有可能可以抑制默认的错误处理程序：

```javascript
// This is a truncated version of the buffer from the above examples
const buffer = new Buffer('eJzT0yMA', 'base64');

zlib.unzip(buffer, {
    finishFlush: zlib.Z_SYNC_FLUSH
}, (err, buffer) => {
    if (!err) {
        console.log(buffer.toString());
    } else {
        // handle error
    }
});
```

这不会改变其他情况下抛出错误的行为，例如，当输入无效格式的数据。使用该方法，这将不可能确定输入是否提前结束或是否缺乏完整性检查，因此有必要手动检查该解压缩结果的有效性。