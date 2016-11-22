# 方法和属性

* [repl.start([options])](#replstartoptions)

--------------------------------------------------

## repl.start([options])

返回并启动一个 `REPLServer` 实例，继承自 [Readline Interface](../readline/class_Interface.md#)。接受具有以下值的“options”对象：

* `prompt` - 用于所有 I/O `stream` 的提示。默认为 `> `。

* `input` - 要监听的可读流。默认为 `process.stdin`。

* `output` - 写入逐行读取数据的写入流。默认为 `process.stdout`。

* `terminal` - 如果 `stream` 应该像一个 `TTY` 对待时，传入 `true`，并写入 ANSI/VT100 转义码。默认在 `output` 流实例化时检测 `isTTY`。

* `eval` - 将用于评估每个给定的行的函数。默认 `eval()` 为异步封装器。参见下面的自定义示例 `eval`。

* `useColors` - 指定 `writer` 函数是否应该输出颜色的布尔值。如果设置了一个不同的 `writer` 函数，那么这将没有效果。默认为该 repl 的 `terminal` 值。

* `useGlobal` - 如果设置为 `true`，那么该 repl 会使用 `global` 对象，代替在单独上下文中运行脚本。默认为 `false`。

* `ignoreUndefined` - 如果设置为 `true`，那么 repl 将不会输出返回值为 `undefined` 的命令。默认为 `false`。

* `writer` - 为每个命令调用的函数，它返回格式化后（包括着色）的显示。默认为 `util.inspect`。

* `replMode` - 控制 repl 是否以严格模式，默认模式或混合模式（“魔术”模式）运行所有命令。可接受的值为：

    - `repl.REPL_MODE_SLOPPY` - 以粗略模式运行命令。
    
    - `repl.REPL_MODE_STRICT` - 以严格模式运行命令。这相当于在每个 repl 语句前面带上 `'use strict'`。
    
    - `repl.REPL_MODE_MAGIC` - 尝试在默认模式下运行命令。如果他们无法解析，请在严格模式下重试。
    
如果有以下签名的情况下，你可以使用你自己的 `eval` 函数：

``` javascript
function eval(cmd, context, filename, callback) {
    callback(null, result);
}
```

选项卡完成时，`eval` 将用 `.scope` 作为输入字符串调用。它期望返回用于自动补全的作用域名称的数组。

多数的 REPLs 可以针对相同的 Node.js 运行实例启动。每个将共享相同的全局对象，但会具有唯一的 I/O。

这里有一个在 stdin、Unix 套接字和 TCP 套接字上运行 REPL 的例子：

``` javascript
const net = require('net');
const repl = require('repl');
var connections = 0;

repl.start({
    prompt: 'Node.js via stdin> ',
    input: process.stdin,
    output: process.stdout
});

net.createServer((socket) => {
    connections += 1;
    repl.start({
        prompt: 'Node.js via Unix socket> ',
        input: socket,
        output: socket
    }).on('exit', () => {
        socket.end();
    })
}).listen('/tmp/node-repl-sock');

net.createServer((socket) => {
    connections += 1;
    repl.start({
        prompt: 'Node.js via TCP socket> ',
        input: socket,
        output: socket
    }).on('exit', () => {
        socket.end();
    });
}).listen(5001);
```

在一个命令行中运行这个程序会在 stdin 中启动一个 REPL。其他的 REPL 客户端，可以通过 Unix 套接字或 TCP 套接字进行连接。`telnet` 对于连接到 TCP 套接字很有用，`socat` 可以用于连接到 Unix 和 TCP 套接字。

通过从基于 Unix 套接字的服务器而不是从 stdin 启动 REPL，你可以连接到长期运行的 Node.js 进程，而无需重新启动它。

在一个 `net.Server` 和 `net.Socket` 的实例上运行“全功能”（`terminal`）REPL 的例子，详见：[https://gist.github.com/2209310](https://gist.github.com/2209310)。

在 `curl(1)` 上运行 REPL 实例的例子，详见：[https://gist.github.com/2053342](https://gist.github.com/2053342)