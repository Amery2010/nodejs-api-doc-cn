# 方法和属性

* [module.id](#moduleid)
* [module.filename](#modulefilename)
* [module.loaded](#moduleloaded)
* [module.parent](#moduleparent)
* [module.children](#modulechildren)
* [module.exports](#moduleexports)
  - [exports 的别名](#exports-的别名)
* [module.require(id)](#modulerequireid)

--------------------------------------------------

## module 对象

添加：v0.1.16

- {Object}

在每一个模块中，自由变量 `module` 是对表示当前模块的对象的引用。为了方便起见，`module.exports` 也可以通过模块全局的 `exports` 对象访问到。`module` 实际上不是全局的，而是每个模块内部的。


## module.id

添加：v0.1.16

- {String}

模块的标识符。通常是完全解析后的文件名。


## module.filename

添加：v0.1.16

- {String}

模块完全解析后的文件名。


## module.loaded

添加：v0.1.16

- {Boolean}

显示模块是否已经加载完成，或正在加载中。


## module.parent

添加：v0.1.16

- {Object} Module 对象

在模块中最先引入的模块。


## module.children

添加：v0.1.16

- {Array}

需要引入该模块的模块对象。


## module.exports

添加：v0.1.16

- {Object}

`module.exports` 对象由模块系统创建。有时候这是难以接受的，许多人都希望他们的模块成为某个类的实例。为了实现这一点，你需要将要导出的对象赋值给 `module.exports`。需要注意的是如果你将需要导出的对象赋值给 `exports` 只会简单的绑定到本地变量 `exports` 上，这很可能不是你想要的结果。

例如，假设我们创建了一个名为 `a.js` 模块：

``` javascript
const EventEmitter = require('events');

module.exports = new EventEmitter();

// 处理一些工作，并在一段时间后从模块自身内部发出 'ready' 事件。
setTimeout(() => {
	module.exports.emit('ready');
}, 1000);
```

然后，在另一个文件中我们可以这么做：

``` javascript
const a = require('./a');
a.on('ready', () => {
	console.log('module a is ready');
});
```

注意，必须立即完成对 `module.expors` 的赋值，而不能在任何回调中完成。这样是不起作用的：

x.js：

``` javascript
setTimeout(() => {
    module.exports = { a: 'hello' };
}, 0);
```

y.js：

``` javascript
const x = require('./x');
console.log(x.a);
```

#### exports 的别名

添加：v0.1.16

变量 `exports` 是模块内部在最开始生成的指向 `module.exports` 的引用。对于任何变量而言，如果你为其赋一个新值，它将不再绑定到以前的值。

为了解释这个情况，我们想象以下对 `require()` 的假设：

``` javascript
function require(...) {
	// ...
	((module, exports) => {
		// 在这里写你的模块代码
		// 重新分配 export，export 不在是 module.exports 的快捷方式，并且不再导出任何内容。
		exports = some_func;
		// 使你的模块导出内容
		module.exports = some_func;
	})(module, module.exports);
	
	return module;
}
```

原则上，如果你理解不了 `exports` 和 `module.exports` 之间的关系，请忽略 `exports`，只使用 `module.exports`。


## module.require(id)

添加：v0.5.1

- `id` {String}

- 返回：{Object} 已解析的模块的 `module.exports`

`module.require` 方法提供了一种像 `require()` 那样从原始模块加载一个模块的方式。

注意，为了做到这一点，你必须获取一个 `module` 对象的引用。因为 `require()` 会返回 `module.exports`，并且 `module` 是一个典型的*只能*在特定模块作用域内有效的变量，如果想要使用它，就必须明确的导出。