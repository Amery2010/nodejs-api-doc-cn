# 方法和属性

* [Buffer 类](#buffer-类)
* [__dirname](#dirname)
* [__filename](#filename)
* [global](#global)
* [process](#process)
* [module](#module)
* [exports](#exports)
* [require()](#require)
  - [require.cache](#requirecache)
  - [require.extensions](#requireextensions) 废弃
  - [require.resolve()](#requireresolve)
* [console](#console)
* [setTimeout(callback, delay[, ...args])](#settimeoutcallback-delay-args)
* [clearTimeout(timeoutObject)](#cleartimeouttimeoutobject)
* [setInterval(callback, delay[, ...args])](#setintervalcallback-delay-args)
* [clearInterval(intervalObject)](#clearintervalintervalobject)
* [setImmediate(callback[, ...args])](#setimmediatecallback-args)
* [clearImmediate(immediateObject)](#clearimmediateimmediateobject)

--------------------------------------------------


## Buffer 类

添加：v0.1.103

- {Function}

用于处理二进制数据。详见 [Buffer](../buffer/) 章节。


## __dirname

添加：v0.1.27

- {String}

当前执行脚本所在的目录名称。

示例：在 `/Users/mjr` 中运行 `node example.js`：

``` javascript
console.log(__dirname);
// 打印: /Users/mjr
```

`__dirname` 实际上不是全局的，而是每个模块内部的。

例如，给出两个模块：`a` 和 `b`，其中 `b` 是 `a` 的依赖文件。目录结构如下：

* `/Users/mjr/app/a.js`

* `/Users/mjr/app/node_modules/b/b.js`

在 `b.js` 中引用 `__dirname` 会返回 `/Users/mjr/app/node_modules/b`，而在 `a.js` 中引用 `__dirname` 则会返回 `/Users/mjr/app`。

*上文的 `/Users/mjr` 指代用户目录，在实际应用中请使用自己的目录路径进行操作，[译者](https://github.com/Amery2010)注。*


## __filename

添加：v0.0.1

- {String}

当前所执行脚本的文件名。这是该脚本文件经过解析后生成的绝对路径。在模块中此变量值是该模块的文件路径。对于主程序而言，这与命令行中使用的文件名未必相同。

示例：在 `/Users/mjr` 中运行 `node example.js`：

``` javascript
console.log(__filename);
// 打印: /Users/mjr/example.js
```

`__filename` 实际上不是全局的，而是每个模块内部的。

*上文的 `/Users/mjr` 指代用户目录，在实际应用中请使用自己的目录路径进行操作，[译者](https://github.com/Amery2010)注。*


## global

添加：v0.1.27

- {Object} 全局命名空间对象。

在浏览器中，顶级作用域就是全局作用域。这也意味着在浏览器中，如果在全局作用域内使用 `var something` 将会声明一个全局变量。在 Node.js 中则不同。顶级作用域并非全局作用域；在 Node.js 模块中使用 `var something` 会生成该模块的一个本地变量。


## process

添加：v0.1.7

- {Object}

进程对象。详见[进程](../process/)章节。


## module

添加：v0.1.16

- {Object}

当前模块的引用。尤其是 `module.exports` 用于定义模块的导出并确保该模块能够通过 `require()` 引入。

`module` 实际上不是全局的，而是每个模块内部的。

有关的详细信息，请参阅[模块系统文档](../modules/)。


## exports

添加：v0.1.12

`module.exports` 的快捷引用方式。何时使用 `exports` 以及何时使用 `module.exports` 的详细内容，请参阅[模块系统文档](../modules/)。

`exports` 实际上不是全局的，而是每个模块内部的。

有关详细信息，请参阅[模块系统文档](../modules/)。


## require()

添加：v0.1.13

- {Function}

引入模块。详见[模块](../modules/)章节。

`require` 实际上不是全局的，而是每个模块内部的。

### require.cache

添加：v0.3.0

- {Object}

模块在引入时会缓存到该对象中。如果从该对象中删除键值，下次调用 `require` 时将重新加载相应模块。请注意，这不适用于[原生插件](./addons/)，因为重新加载这类插件将导致错误。

### require.extensions

添加：v0.3.0  废弃：v0.10.6

> 稳定度：0 - 已废弃

- {Object}

用于指导 `require` 方法如何处理特定的文件扩展名。

将带有扩展名 `.sjs` 文件作为 `.js` 文件处理：

``` javascript
require.extensions['.sjs'] = require.extensions['.js'];
```

在 `废弃` 之前，该列表用于将通过按需编译的非 JavaScript 模块加载到 Node.js 中。然而，实践中有更好的方式实现这一点，比如通过其他的 Node.js 程序来加载模块，或预先将它们编译成 JavaScript 代码。

由于[模块系统](../modules/)被锁定，该特性可能永远不会消失。改动它可能会产生细微的错误和复杂性，所以最好保持不变。

请注意，模块系统必须执行与已注册的扩展数成线性比例的文件系统操作次数，才能将一个 `require(...)` 语句解析为文件名。

换句话说，添加扩展会降低模块加载器的执行效率，不鼓励使用该方法。


### require.resolve()

添加：v0.3.0

该方法使用 `require()` 的内部机制来查找模块的位置，但不会加载该模块，只返回解析后的文件名。


## console

添加：v0.1.100

- {Object}

用于打印 `stdout` 和 `stderr`。详见[控制台](../console/)章节。


## setTimeout(callback, delay[, ...args])

添加：v0.0.1

有关 [setTimeout](../timers/scheduling_timers.md#settimeoutcallback-delay-args) 的描述在[定时器](../timers/)章节。


## clearTimeout(timeoutObject)

添加：v0.0.1

有关 [clearTimeout](../timers/cancelling_timers.md#cleartimeouttimeout) 的描述在[定时器](../timers/)章节。


## setInterval(callback, delay[, ...args])

添加：v0.0.1

有关 [setInterval](../timers/scheduling_timers.md#setintervalcallback-delay-args) 的描述在[定时器](../timers/)章节。


## clearInterval(intervalObject)

添加：v0.0.1

有关 [clearInterval](../timers/cancelling_timers.md#clearintervaltimeout) 的描述在[定时器](../timers/)章节。


## setImmediate(callback[, ...args])

添加：v0.9.1

有关 [setImmediate](../timers/scheduling_timers.md#setimmediatecallback-args) 的描述在[定时器](../timers/)章节。


## clearImmediate(immediateObject)

添加：v0.9.1

有关 [clearImmediate](../timers/cancelling_timers.md#clearimmediateimmediate) 的描述在[定时器](../timers/)章节。