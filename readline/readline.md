# 方法和属性

#### 事件

* ['SIGINT' 事件](#sigint-事件)
* ['SIGCONT' 事件](#sigcont-事件)
* ['SIGTSTP' 事件](#sigtstp-事件)
* ['line' 事件](#line-事件)
* ['pause' 事件](#pause-事件)
* ['resume' 事件](#resume-事件)
* ['close' 事件](#close-事件)

#### 方法

* [readline.createInterface(options)](#readlinecreateInterfaceoptions)
* [readline.cursorTo(stream, x, y)](#readlinecursortostream-x-y)
* [readline.moveCursor(stream, dx, dy)](#readlinemovecursorstream-dx-dy)
* [readline.clearLine(stream, dir)](#readlineclearLinestream-dir)
* [readline.clearScreenDown(stream)](#readlineclearscreendownstream)

-------------------------------------------------------


## 'SIGINT' 事件

`function () {}`

每当 `input` 流接收到 `^C`（即，`SIGINT`）时发出。当 `input` 流接收到 `SIGINT` 时，如果没有 `SIGINT` 事件监听器存在，将会触发 `pause`。

监听 `SIGINT` 的例子：

``` javascript
rl.on('SIGINT', () => {
    rl.question('Are you sure you want to exit?', (answer) => {
        if (answer.match(/^y(es)?$/i)) rl.pause();
    });
});
```


## 'SIGCONT' 事件

`function () {}`

**这在 Windows 上不起作用。**

每当 `input` 流发送一个 `^Z`（即，`SIGTSTP`）到后台时发出，然后继续 `fg(1)`。此事件仅在将程序发送到后台之前流未被暂停时发出。

监听 `SIGCONT` 的例子：

``` javascript
rl.on('SIGCONT', () => {
    // `prompt` will automatically resume the stream
    rl.prompt();
});
```


## 'SIGTSTP' 事件

`function () {}`

**这在 Windows 上不起作用。**

每当 `input` 流接收到 `^Z`（即，`SIGTSTP`）时发出。当 `input` 流接收到 `SIGTSTP` 时，如果没有 `SIGTSTP` 事件监听器存在，该程序将被发送到后台。

当程序通过 `fg` 恢复时，会发出 `'pause'` 和 `SIGCONT` 事件。你也可以用来恢复流。

如果流在程序发送到后台之前暂停，将不会发出 `'pause'` 和 `SIGCONT` 事件。

监听 `SIGTSTP` 的例子：

``` javascript
rl.on('SIGTSTP', () => {
    // This will override SIGTSTP and prevent the program from going to the
    // background.
    console.log('Caught SIGTSTP.');
});
```


## 'line' 事件

`function (line) {}`

每当 `input` 流接收到接收行结束符（`\n`、`\r` 或 `\r\n`）时发出，通常在用户点击进入或返回时接收。这是一个很好的 hook，可以用来监听用户输入。

监听 `line` 的例子：

``` javascript
rl.on('line', (cmd) => {
    console.log(`You just typed: ${cmd}`);
});
```


## 'pause' 事件

`function () {}`

每当 `input` 流暂停时发出。

同时也在每当 `input` 流没有暂停并接收到 `SIGCONT` 事件时发出。（详见 `SIGTSTP` 和 `SIGCONT` 事件）

监听 `pause` 的例子：

``` javascript
rl.on('pause', () => {
    console.log('Readline paused.');
});
```


## 'resume' 事件

`function () {}`

每当 `input` 流恢复时发出。

监听 `resume` 的例子：

``` javascript
rl.on('resume', () => {
    console.log('Readline resumed.');
});
```


## 'close' 事件

`function () {}`

当调用 `close()` 时发出。

同时也在 `input` 流接收到它的 `'end'` 事件时发出。一旦发出，`Interface` 实例应该被认为已经“完成”。举个例子，当 `input` 流接收到 `^D`（即，`EOT`）时。

当 `input` 流接收到 `^C`（即，`SIGINT`），如果不存在 `SIGINT` 事件监听器时也会被调用。


## readline.createInterface(options)

创建一个 readline 的 `Interface` 实例。接收一个有以下值的 `options` 对象：

* `input` - 监听的可读流。（必需）

* `output` - 写入 readline 数据的可写流。（可选）

* `completer` - 一个用于 Tab 自动补全的可选函数。请参见下面的使用示例。

* `terminal` - 如果 `input` 和 `output` 流应该像 TTY 一样对待时传入 `true`，并会写入 ANSI/VT100 转义码。默认在 `output` 流实例化时检测 `isTTY`。

* `historySize` - 保留的最大历史记录行数。默认为 `30`。

`completer` 函数给定用户输入的当前行，并应该返回具有 2 个条目的数组：

1. 用于补全的匹配条目数组。

2. 用于匹配的子字符串。

最后看起来像：`[[substr1, substr2, ...], originalsubstring]`。

例子：

``` javascript
function completer(line) {
    var completions = '.help .error .exit .quit .q'.split(' ')
    var hits = completions.filter((c) => {
            return c.indexOf(line) == 0
        })
        // show all completions if none found
    return [hits.length ? hits : completions, line]
}
```

如果它接受两个参数，`completer` 也可以在异步模式下运行：

``` javascript
function completer(linePartial, callback) {
    callback(null, [['123'], linePartial]);
}
```

`createInterface` 通常与 [process.stdin](../process/process.md#stdin) 和 [process.stdout](../process/process.md#stdout) 一起使用以便接受用户输入：

``` javascript
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});
```

一旦你有一个 readline 实例，你最常见的是去监听 `'line'` 事件。

如果在这个实例中 `terminal` 为 `true`，那么 `output` 流将获得最佳的兼容性。如果它定义了一个 `output.columns` 属性，如果/当列发生变化时，会在 `output` 上触发一个 `'resize'` 事件。（当 [process.stdout](../process/process.md#stdout) 是 TTY 时自动执行此操作）


## readline.cursorTo(stream, x, y)

将光标移动到给定 TTY 流中的指定位置。


## readline.moveCursor(stream, dx, dy)

将光标相对于其在给定 TTY 流中的当前位置进行移动。


## readline.clearLine(stream, dir)

清除给定 TTY 流在指定方向上的当前行。`dir` 应该具有以下值之一：

* `-1` - 从光标向左移动

* `1` - 从光标向右移动

* `0` - 整行


## readline.clearScreenDown(stream)

从光标的当前位置开始清空屏幕。