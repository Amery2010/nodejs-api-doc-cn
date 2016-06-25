# Console类

* [new Console(stdout[, stderr])](#new_Console)
* [console.log([data][, ...])](#log)
* [console.info([data][, ...])](#info)
* [console.error([data][, ...])](#error)
* [console.warn([data][, ...])](#warn)
* [console.dir(obj[, options])](#dir)
* [console.trace(message[, ...])](#trace)
* [console.assert(value[, message][, ...])](#assert)
* [console.time(label)](#time)
* [console.timeEnd(label)](#timeEnd)

--------------------------------------------------


`Console` 类可用于创建具有可配置的输出流的简单记录，也可以使用 `require('console').Console` 或 `console.Console` 替代。

```javascript
const Console = require('console').Console;
const Console = console.Console;
```


<div id="new_Console" class="anchor"></div>
## new Console(stdout[, stderr])

通过一个或两个可写流实例创建一个新的 `Console` 对象。`stdout` 是一个可写流用于打印日志或信息输出。`stderr` 用于输出警告或错误信息。如果 `stderr` 没有正常输出，则警告或错误将被发送到 `stdout` 。

```javascript
const output = fs.createWriteStream('./stdout.log');
const errorOutput = fs.createWriteStream('./stderr.log');
// custom simple logger
const logger = new Console(output, errorOutput);
// use it like console
var count = 5;
logger.log('count: %d', count);
// in stdout.log: count 5
```

全局的 `console` 是一个将输出发送到 [process.stdout](../process/process.md#stdout) 和 [process.stderr](../process/process.md#stderr) 的特殊的 `Console` 实例。这等同于调用：

```javascript
new Console(process.stdout, process.stderr);
```


<div id="log" class="anchor"></div>
## console.log([data][, ...])

在新行里打印 `stdout` 。可以传多个参数，第一个作为主要信息，其余的参数作为替换值通过 `printf(3)` 输出（这些参数使用 [util.format()](../util/util.md#format) 处理）

```javascript
var count = 5;
console.log('count: %d', count);
  // Prints: count: 5, to stdout
console.log('count: ', count);
  // Prints: count: 5, to stdout
```

如果在第一个字符串中没有找到格式化元素（如，`%d`），那么 [util.inspect()](../util/util.md#inspect) 将被应用到各个参数。详见 [util.format()](../util/util.md#format)。


<div id="info" class="anchor"></div>
## console.info([data][, ...])

`console.info()` 函数是 [console.log()](#log) 的别名。


<div id="error" class="anchor"></div>
## console.error([data][, ...])

在新行里打印 `stderr` 。可以传多个参数，第一个作为主要信息，其余的参数作为替换值通过 `printf(3)` 输出（这些参数使用 [util.format()](../util/util.md#format) 处理）

```javascript
const code = 5;
console.error('error #%d', code);
  // Prints: error #5, to stderr
console.error('error', code);
  // Prints: error 5, to stderr
```

如果在第一个字符串中没有找到格式化元素（如，`%d`），那么 [util.inspect()](../util/util.md#inspect) 将被应用到各个参数。详见 [util.format()](../util/util.md#format)。


<div id="warn" class="anchor"></div>
## console.warn([data][, ...])

`console.warn()` 函数是 [console.error()](#error) 的别名。


<div id="dir" class="anchor"></div>
## console.dir(obj[, options])

该函数在 `obj` 上使用 [util.inspect()](../util/util.md#inspect) 并将结果字符串输出到 `stdout` 中。此函数会绕过任何定义在 `obj` 上的 `inspect()` 方法。可选 `options` 对象可以传一些格式化的字符串：

- `showHidden` - 如果为 `true` 则该对象的不可枚举属性和 `Symbol` 属性也将被显示，默认为 `false` 。

- `depth` - 告知函数在递归格式化对象时使用多少次 `util.inspect()` ，这在检查大的复杂的对象时非常有用，默认值为 2 。可以通过设置为 `null` 来实现无限递归。

- `colors` - 如果为 `true` 则输出结果将使用 ANSI 颜色代码，默认值为 `false`。颜色可定制，详见 [定制 util.inspect() 颜色](../util/util.md#customizing_util_inspect_colors)。


<div id="trace" class="anchor"></div>
## console.trace(message[, ...])

通过 `stderr` 打印字符串 `'Trace :'`，随后跟着被 [util.format()](../util/util.md#format) 格式化后的信息以及堆栈跟踪代码的当前位置。

```javascript
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


<div id="assert" class="anchor"></div>
## console.assert(value[, message][, ...])

一个简单的断言测试验证 `value` 是否为真值。如果不是则抛出一个 `AssertionError` 。如果提供错误 `message` 参数则使用 [util.format()](../util/util.md#format) 格式化并作为错误信息输出。

```javascript
console.assert(true, 'does nothing');
  // OK
console.assert(false, 'Whoops %s', 'didn\'t work');
  // AssertionError: Whoops didn't work
```


<div id="time" class="anchor"></div>
## console.time(label)

启动一个定时器用以计算一个操作的持续时间。定时器由一个唯一标识的 `label` 确定。通过 [console.timeEnd()](#timeEnd) 使用相同的 `label` 来停止一个定时器并输出以毫秒为单位的持续时间到 `stdout` ，持续时间是精确到亚毫秒的。


<div id="timeEnd" class="anchor"></div>
## console.timeEnd(label)

停止先前通过 [console.time()](#time) 启动的定时器并打印结果到 `stdout` ：

```javascript
console.time('100-elements');
for (var i = 0; i < 100; i++) {
    ;
}
console.timeEnd('100-elements');
// prints 100-elements: 225.438ms
```
