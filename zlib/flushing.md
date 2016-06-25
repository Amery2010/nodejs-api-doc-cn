# Flushing

在一个压缩流中调用 [.flush()](./zlib_class.md#flush) 会使得 zlib 尽可能多地返回当前的可能值。这可能会降低压缩质量的成本，但这在数据需要尽快使用时非常有用。

在以下的例子中，`flush()` 被用于在客户端写入一个部分压缩的 HTTP 响应：

```javascript
const zlib = require('zlib');
const http = require('http');

http.createServer((request, response) => {
    // For the sake of simplicity, the Accept-Encoding checks are omitted.
    response.writeHead(200, {
        'content-encoding': 'gzip'
    });
    const output = zlib.createGzip();
    output.pipe(response);

    setInterval(() => {
        output.write(`The current time is ${Date()}\n`, () => {
            // The data has been passed to zlib, but the compression algorithm may
            // have decided to buffer the data for more efficient compression.
            // Calling .flush() will make the data available as soon as the client
            // is ready to receive it.
            output.flush();
        });
    }, 1000);
}).listen(1337);
```