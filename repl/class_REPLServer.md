# REPLServer类

* ['reset' 事件](#reset-事件)
* ['exit' 事件](#exit-事件)
* [replServer.defineCommand(keyword, cmd)](#replserverdefinecommandkeyword-cmd)
* [replServer.displayPrompt([preserveCursor])](#replserverdisplaypromptpreservecursor)

--------------------------------------------------

它继承自 [Readline Interface](../readline/class_Interface.md#) 并带有以下事件：


## 'reset' 事件

`function (context) {}`

当 REPL 的上下文被重置时发出。这在你键入 `.clear` 时发生。如果你启动时带有 `{ useGlobal: true }`，那么这个事件永远不会被触发。

监听 `'reset'` 事件的例子：

``` javascript
// Extend the initial repl context.
var replServer = repl.start({
    options...
});
someExtension.extend(r.context);

// When a new context is created extend it as well.
replServer.on('reset', (context) => {
    console.log('repl has a new context');
    someExtension.extend(context);
});
```


## 'exit' 事件

`function () {}`

当用户以任何定义的方式退出 REPL 时发出。也就是说，在 repl 中键入 `.exit`，按下 `Ctrl+C` 两次来示意 `SIGINT`，或在 `input` 流中，按下 `Ctrl+D` 来示意 `'end'`。

监听 `'exit'` 事件的例子：

``` javascript
replServer.on('exit', () => {
    console.log('Got "exit" event from repl!');
    process.exit();
});
```


## replServer.defineCommand(keyword, cmd)

* `keyword` {String}

* `cmd` {Object} | {Function}

使一个命令在 REPL 中可用。该命令通过键入 `.` 后面跟着关键字来调用。该 `cmd` 是一个具有以下值的对象：

* `help` - 当输入 `.help` 时显示的帮助文本信息（可选）。

* `action` - 一个要执行的函数，可能可以接受字符串参数，当调用命令时，绑定到 REPL 服务器实例上（必需）。

如果提供了一个函数而不是一个 `cmd` 对象，它被视为 `action`。

定义命令的例子：

``` javascript
// repl_test.js
const repl = require('repl');

var replServer = repl.start();
replServer.defineCommand('sayhello', {
    help: 'Say hello',
    action: function (name) {
        this.write(`Hello, ${name}!\n`);
        this.displayPrompt();
    }
});
```

从 REPL 中调用该命令的例子：

``` bash
> .sayhello Node.js User
Hello, Node.js User!
```


## replServer.displayPrompt([preserveCursor])

* `preserveCursor` {Boolean}

像 [readline.prompt](../readline/class_Interface.md#rlpromptpreservecursor) 那样，当在块内时，添加缩进和省略号。`preserveCursor` 参数是传给 [readline.prompt](../readline/class_Interface.md#rlpromptpreservecursor) 的。这主要用于 `defineCommand`。它也用于内部渲染每个提示行。