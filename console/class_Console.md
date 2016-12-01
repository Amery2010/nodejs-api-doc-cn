# Console 类

* [new Console(stdout[, stderr])](#new-consolestdout-stderr)
* [console.log([data][, ...args])](#consolelogdata-args)
* [console.info([data][, ...args])](#consoleinfodata-args)
* [console.error([data][, ...args])](#consoleerrordata-args)
* [console.warn([data][, ...args])](#consolewarndata-args)
* [console.dir(obj[, options])](#consoledirobj-options)
* [console.trace(message[, ...args])](#consoletracemessage-args)
* [console.assert(value[, message][, ...args])](#consoleassertvalue-message-args)
* [console.time(label)](#consoletimelabel)
* [console.timeEnd(label)](#consoletimeendlabel)

--------------------------------------------------

`Console` 类可用于创建具有可配置的输出流的简单记录器，也可以使用 `require('console').Console` 或 `console.Console`：

``` javascript
const Console = require('console').Console;
const Console = console.Console;
```


## new Console(stdout[, stderr])

通过一个或两个可写流实例创建一个新的 `Console` 对象。`stdout` 是一个用于打印日志或信息输出的可写流。`stderr` 用于输出警告或错误信息。如果 `stderr` 没有正常输出，警告或错误将被发送到 `stdout` 。

``` javascript
const output = fs.createWriteStream('./stdout.log');
const errorOutput = fs.createWriteStream('./stderr.log');
// 自定义的简单记录器
const logger = new Console(output, errorOutput);
// 像 console 那样使用
var count = 5;
logger.log('count: %d', count);
// 在 stdout.log 中打印: count 5
```

全局的 `console` 是一个特殊的 `Console` 实例，它的输出会发送到 [process.stdout](../process/process.md#stdout) 和 [process.stderr](../process/process.md#stderr)。它相当于调用：

``` javascript
new Console(process.stdout, process.stderr);
```


## console.log([data][, ...args])

添加：v0.1.100

使用换行符将信息打印到 `stdout`。可以传多个参数，第一个作为主要信息，其余的参数作为类似于 `printf(3)` 中的替换值（这些参数都会传给 [util.format()](../util/util.md#format) 进行处理）。

``` javascript
var count = 5;
console.log('count: %d', count);
// 在 stdout 中打印: count: 5
console.log('count:', count);
// 在 stdout 中打印: count: 5
```

如果在第一个字符串中没有找到格式化元素（如，`%d`），那么 [util.inspect()](../util/util.md#inspect) 会在每个参数上调用并将结果字符串值拼在一起。详见 [util.format()](../util/util.md#format)。


## console.info([data][, ...args])

添加：v0.1.100

`console.info()` 函数是 [console.log()](#consolelogdata-args) 的别名。


## console.error([data][, ...args])

添加：v0.1.100

使用换行符将信息打印到 `stderr`。可以传多个参数，第一个作为主要信息，其余的参数作为类似于 `printf(3)` 中的替换值（这些参数都会传给 [util.format()](../util/util.md#format) 进行处理）。

``` javascript
const code = 5;
console.error('error #%d', code);
// 在 stderr 中打印: error #5
console.error('error', code);
// 在 stderr 中打印: error 5
```

如果在第一个字符串中没有找到格式化元素（如，`%d`），那么 [util.inspect()](../util/util.md#inspect) 会在每个参数上调用并将结果字符串值拼在一起。详见 [util.format()](../util/util.md#format)。


## console.warn([data][, ...args])

添加：v0.1.100

`console.warn()` 函数是 [console.error()](#consoleerrordata-args) 的别名。


## console.dir(obj[, options])

添加：v0.1.101

该函数在 `obj` 上使用 [util.inspect()](../util/util.md#inspect) 并将结果字符串打印到 `stdout` 中。此函数会绕过任何定义在 `obj` 上的 `inspect()` 方法。可选 `options` 对象可以传一些用于格式化字符串的内容：

* `showHidden` - 如果为 `true`，那么该对象中的不可枚举属性和 `Symbol` 属性也会显示，默认为 `false`。

* `depth` - 告诉 `util.inspect()` 函数在格式化对象时递归多少次。这对于检查大型复杂对象很有用。默认为 `2`。可以通过设置为 `null` 来实现无限递归。

* `colors` - 如果为 `true`，那么输出结果将使用 ANSI 颜色代码。默认为 `false`。这里的颜色是可定制的，详见[定制 util.inspect() 颜色](../util/util.md#customizing_util_inspect_colors)。


## console.trace(message[, ...args])

添加：v0.1.104

通过 `stderr` 打印字符串 `'Trace :'`，以及后面通过 [util.format()](../util/util.md#format) 格式化的消息和堆栈跟踪在代码中的当前位置。

``` javascript
console.trace('Show me');
// Prints: (stack trace will vary based on where trace is called)
//  Trace: Show me
//    at repl:2:9
//    at REPLServer.defaultEval (repl.js:248:27)
//    at bound (domain.js:287:14)
//    at REPLServer.runBound [as eval] (domain.js:300:12)
//    at REPLServer.<anonymous> (repl.js:412:12)
//    at emitOne (events.js:82:20)
//    at REPLServer.emit (events.js:169:7)
//    at REPLServer.Interface._onLine (readline.js:210:10)
//    at REPLServer.Interface._line (readline.js:549:8)
//    at REPLServer.Interface._ttyWrite (readline.js:826:14)
```


## console.assert(value[, message][, ...args])

添加：v0.1.101

一个简单的断言测试，验证 `value` 是否为真。如果不是，则抛出一个 `AssertionError`。如果提供错误 `message` 参数，则使用 [util.format()](../util/util.md#format) 格式化并作为错误信息输出。

``` javascript
console.assert(true, 'does nothing');
// OK
console.assert(false, 'Whoops %s', 'didn\'t work');
// AssertionError: Whoops didn't work
```

*注意：`console.assert()` 方法在 Node.js 中的实现方式和[用在浏览器中](https://developer.mozilla.org/zh-CN/docs/Web/API/Console/assert)的 `console.assert()` 方法是不一样的。*

特别是在浏览器中调用假的断言后，`console.assert()` 会导致 `message` 被打印到控制台，但不会中断后续代码的执行。而在 Node.js 中，一个假的断言将导致抛出一个 `AssertionError` 错误。

可以通过扩展 Node.js 的 `console` 并覆盖 `console.assert()` 方法来实现浏览器中实现的类似功能。

在以下示例中，会创建一个简单的模块，该模块扩展并覆盖了 Node.js 中 `console` 的默认行为。

``` javascript
'use strict';

// 用一种新的没有采用猴子补丁实现的 assert 来创建一个简单的 console 扩展。
const myConsole = Object.setPrototypeOf({
    assert(assertion, message, ...args) {
        try {
            console.assert(assertion, message, ...args);
        } catch (err) {
            console.error(err.stack);
        }
    }
}, console);

module.exports = myConsole;
```

然后可以用于直接替换内置的控制台：

``` javascript
const console = require('./myConsole');
console.assert(false, 'this message will print, but no error thrown');
console.log('this will also print');
```


## console.time(label)

添加：v0.1.104

启动一个定时器用以计算一个操作的持续时间。定时器由唯一的 `label` 标识。当调用 [console.timeEnd()](#timeEnd) 时，可以使用相同的 `label` 来停止定时器并以毫秒为单位将持续时间输出到 `stdout`。定时器持续时间精确到亚毫秒。


## console.timeEnd(label)

添加：v0.1.104

停止之前通过 [console.time()](#time) 启动的定时器并将结果打印到 `stdout`：

``` javascript
console.time('100-elements');
for (var i = 0; i < 100; i++) {
    ;
}
console.timeEnd('100-elements');
// 打印 100-elements: 225.438ms
```

*注意：从 Node.js v6.0.0 开始，`console.timeEnd()` 删除了计时器以避免泄漏。在旧版本上，依然保留着计时器，这会允许对同一标签调用 `console.timeEnd()` 多次。此功能是非预期的，不再受到支持。*