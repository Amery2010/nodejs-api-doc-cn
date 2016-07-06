# 文件系统(File System)

> 稳定度：2 - 稳定

文件 I/O 是由简单封装的标准 POSIX 函数提供。使用该模块使用 `require('fs')`。所有的方法都异步和同步形式。

异步形式始终以完成时的回调作为最后一个参数。传递给完成时的回调的参数取决于方法，但第一个参数总是留给异常。如果操作成功完成，则第一个参数将是 `null` 或 `undefined`。

当使用异步形式时，任何的异常都会被立即抛出。你可以使用 try/catch 来处理异常，或让它们冒泡。

这里是异步版本的例子：

``` javascript
const fs = require('fs');

fs.unlink('/tmp/hello', (err) => {
    if (err) throw err;
    console.log('successfully deleted /tmp/hello');
});
```

这里是同步版本的例子：

``` javascript
const fs = require('fs');

fs.unlinkSync('/tmp/hello');
console.log('successfully deleted /tmp/hello');
```

异步方法没法保证执行顺序。所以下面例子容易出错：

``` javascript
fs.rename('/tmp/hello', '/tmp/world', (err) => {
    if (err) throw err;
    console.log('renamed complete');
});
fs.stat('/tmp/world', (err, stats) => {
    if (err) throw err;
    console.log(`stats: ${JSON.stringify(stats)}`);
});
```

`fs.stat` 也可以在 `fs.rename` 之前执行。要做到这一点，正确的方法是采用回调链。

``` javascript
fs.rename('/tmp/hello', '/tmp/world', (err) => {
    if (err) throw err;
    fs.stat('/tmp/world', (err, stats) => {
        if (err) throw err;
        console.log(`stats: ${JSON.stringify(stats)}`);
    });
});
```

在忙碌的进程中，*强烈推荐*开发者使用这些函数的异步版本。同步版本将会阻止整个进程，直到他们完成——停止所有连接。

可使用文件名的相对路径。但请记住，该路径将相对 `process.cwd()`。

大多数 fs 函数，让你忽略回调参数。如果你这么做，一个默认的回调将用于抛出错误。为了得到一个到原来的调用点的跟踪，设置 `NODE_DEBUG` 环境变量：

``` bash
$ cat script.js
function bad() {
  require('fs').readFile('/');
}
bad();

$ env NODE_DEBUG=fs node script.js
fs.js:66
        throw err;
              ^
Error: EISDIR, read
    at rethrow (fs.js:61:21)
    at maybeCallback (fs.js:79:42)
    at Object.fs.readFile (fs.js:153:18)
    at bad (/path/to/script.js:2:17)
    at Object.<anonymous> (/path/to/script.js:5:1)
    <etc.>
```