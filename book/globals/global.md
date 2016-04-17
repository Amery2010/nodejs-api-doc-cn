# 方法和属性

* [Buffer类](#class_Buffer)
* [__dirname](#dirname)
* [__filename](#filename)
* [global](#global)
* [process](#process)
* [module](#module)
* [exports](#exports)
* [require()](#require)
  - [require.cache](#require_cache)
  - [require.extensions](#require_extensions)
  - [require.resolve()](#require_resolve)
* [console](#console)
* [setTimeout(cb, ms)](#setTimeout)
* [clearTimeout(t)](#clearTimeout)
* [setInterval(cb, ms)](#setInterval)
* [clearInterval(t)](#clearInterval)

--------------------------------------------------


<div id="class_Buffer" class="anchor"></div>
## Buffer类

- {Function}

用于处理二进制数据，详见 [Buffer](../buffer/) 章节。


<div id="dirname" class="anchor"></div>
## __dirname

- {String}

当前执行脚本所在目录的目录名。

举个栗子：在 `/Users/mjr` （此处为你的项目目录，[译者](https://github.com/Amery2010)注）下运行 `node example.js` 。

``` javascript
console.log(__dirname);
// /Users/mjr
```

`__dirname` 实际上是各个模块本地的而非全局。


<div id="filename" class="anchor"></div>
## __filename

- {String}

当前所执行脚本的文件路径。这是该脚本文件经过解析后生成的绝对路径，在模块中此变量值是该模块的文件路径。对于主程序来说，这和命令行中显示的文件路径未必相同。

举个栗子：在 `/Users/mjr` （此处为你的项目目录，[译者](https://github.com/Amery2010)注）下运行 `node example.js` 。

``` javascript
console.log(__filename);
// /Users/mjr/example.js
```

`__filename` 实际上是各个模块本地的而非全局。


<div id="global" class="anchor"></div>
## global

- {Object} 全局命名空间对象

在浏览器中，顶级作用域就是全局作用域。这就是说，在浏览器中如果当前是在全局作用域内，`var something` 将会声明一个全局变量。在 Node 中则不同。顶级作用域并非全局作用域，在 Node 模块里的 `var something` 只属于那个模块。


<div id="process" class="anchor"></div>
## process

- {Object}

进程对象。详见[进程](../process/)章节。


<div id="module" class="anchor"></div>
## module

- {Object}

当前模块的引用。`module.exports` 用于导出模块代码并确保模块能够通过 `require()` 导入。

`module` 实际上是各个模块本地的而非全局。

详情可查阅[模块系统文档](../modules/)。


<div id="exports" class="anchor"></div>
## exports

`module.exports` 的快捷引用方式。何时使用 `exports` 以及何时使用 `module.exports` 的详细内容可查阅[模块系统文档](../modules/)。

`exports` 实际上是各个模块本地的而非全局。

详情可查阅[模块系统文档](../modules/)。


<div id="require" class="anchor"></div>
## require()

- {Function}

引入模块，详见[模块](../modules/)章节。

`require()` 实际上是各个模块本地的而非全局。

<div id="require_cache" class="anchor"></div>
#### require.cache

- {Object}

模块在引入时会缓存到该对象。如果删除该对象的键值，下次调用require时将会重新加载相应模块。

<div id="require_extensions" class="anchor"></div>
#### require.extensions

> 稳定度：0 - 已废弃

- {Object}

用于指导 `require` 方法如何处理特定的文件扩展名。

将 `.sjs` 文件作为 `.js` 文件处理：

``` javascript
require.extensions['.sjs'] = require.extensions['.js'];
```

在 `废弃` 之前，该列表用于编译加载 Node.js 的非 JavaScript 模块。然而，实践中有更好的方式实现该功能，比如通过其他 Node.js 程序加载模块，或提前将他们编译成 JavaScript 代码。

由于[模块系统](../modules/)被锁定，该特性可能永远不会消失。改动它可能会产生细微的错误和复杂性，所以最好保持不变。

<div id="require_resolve" class="anchor"></div>
#### require.resolve()

该方法使用 `require()` 的内部机制查找一个模块的位置，但并不会加载该模块，只返回解析后的文件名。


<div id="console" class="anchor"></div>
## console

- {Object}

用于打印 `stdout` 和 `stderr` 。详见[控制台](../console/)章节。


<div id="setTimeout" class="anchor"></div>
## setTimeout(cb, ms)

在至少 `ms` 毫秒后调用回调函数 `cb` 。实际延迟取决于外部因素，如操作系统定时器粒度及系统负载。

超时值必须在 1-2147483647 的范围内（包含1和2147483647）。如果该值超出范围，则被当作1毫秒处理。一般来说，一个定时器不能超过24.8天。

返回一个代表该定时器的句柄值。


<div id="clearTimeout" class="anchor"></div>
## clearTimeout(t)

停止一个之前通过 [setTimeout()](#setTimeout) 创建的定时器。回调将不再被执行。


<div id="setInterval" class="anchor"></div>
## setInterval(cb, ms)

每隔 `ms` 毫秒重复调用回调函数 `cb` 。值得注意的是，实际间隔取决于外部因素，如操作系统定时器粒度及系统负载。它绝不会少于 `ms` 但可能比 `ms` 长。

超时值必须在 1-2147483647 的范围内（包含1和2147483647）。如果该值超出范围，则被当作1毫秒处理。一般来说，一个定时器不能超过24.8天。

返回一个代表该定时器的句柄值。


<div id="clearInterval" class="anchor"></div>
## clearInterval(t)

停止一个之前通过 [clearInterval()](#clearInterval) 创建的定时器。回调将不再被执行。

定时器函数是一个全局变量。详见[定时器](../timers/)章节。