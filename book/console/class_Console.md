# Console类

* [new Console(stdout[, stderr])](#new_Console)
* [console.log([data][, ...])](#log)
* [console.dir(obj[, options])](#dir)
* [console.info([data][, ...])](#info)
* [console.warn([data][, ...])](#warn)
* [console.error([data][, ...])](#error)
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

全局的 `console` 是一个将输出发送到 [process.stdout](../process/process.md#stdout) 和 [process.stderr](../process/process.md#stderr) 的特殊的 `Console` 实例。
