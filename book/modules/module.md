# 方法和属性

* [module.exports](#exports)
  - [exports的别名](#exports_alias)
* [module.require(id)](#require)
* [module.id](#id)
* [module.filename](#filename)
* [module.loaded](#loaded)
* [module.parent](#parent)
* [module.children](#children)

--------------------------------------------------

## module对象

- {Object}

在每一个模块中，自定义的 `module` 变量是一个当前模块对象的引用。`module.exports` 可以方便地通过模块内的 `exports` 对象访问到。 `module` 事实上不是全局的，更像是每个模块内部的。


<div id="exports" class="anchor"></div>
## module.exports

- {Object}

`module.exports` 对象是通过模块系统产生的。有时候这是难以接受的，许多人想让他们的模块是某个类的实例。为了实现这一点，你需要将要导出的对象赋值给 `module.exports` 。需要注意的是，如果将需要导出的对象赋值给 `exports` 只会简单的绑定到本地变量 `exports` 上，这很可能不是你想要的结果。

例如，假设我们有一个名为 `a.js` 模块：

```javascript
const EventEmitter = require('events');

module.exports = new EventEmitter();

// Do some work, and after some time emit
// the 'ready' event from the module itself.
setTimeout(() => {
    module.exports.emit('ready');
}, 1000);
```

那么，在另一个文件中我们可以这样写

```javascript
const a = require('./a');

a.on('ready', () => {
    console.log('module a is ready');
});
```

值得注意的是，给 `module.expors` 的赋值必须立即生效，不能在回调中执行。这样是不起作用的：

x.js：

```javascript
setTimeout(() => {
    module.exports = { a: 'hello' };
}, 0);
```

y.js：

```javascript
const x = require('./x');
console.log(x.a);
```


<div id="exports_alias" class="anchor"></div>
#### exports的别名

变量 `exports` 是模块内部在最开始生成的指向 `module.exports` 的引用。对于任何变量而言，如果你为其赋一个新值，它将不再绑定到以前的值。

为了解释这个情况，我们想象以下对 `require()` 的假设：

```javascript
function require(...) {
    // ...
    ((module, exports) => {
        // Your module code here
        exports = some_func; // re-assigns exports, exports is no longer
        // a shortcut, and nothing is exported.
        module.exports = some_func; // makes your module export 0
    })(module, module.exports);
    
    return module;
}
```

原则上，如果你理解不了 `exports` 和 `module.exports` 之间的关系，请忘了 `exports` ，仅使用 `module.exports` 。


<div id="require" class="anchor"></div>
## module.require(id)

- `id` {String}

- 返回：{Object} 已解析模块的 `module.exports`

`module.require` 方法提供了一种像 `require()` 一样从最初的模块加载一个模块的方法。

注意，为了做到这一点，你必须获取一个 `module` 对象的引用。 `require()` 返回 `module.exports` ，并且 `module` 是一个典型的*只能*在特定模块作用域内有效的变量，如果想要使用它，就必须明确的导出。


<div id="id" class="anchor"></div>
## module.id

- {String}

用于区别模块的标识符。通常是完全解析后的文件名。


<div id="filename" class="anchor"></div>
## module.filename

- {String}

模块完全解析后的文件名。


<div id="loaded" class="anchor"></div>
## module.loaded

- {Boolean}

显示模块是否已经加载完成，或者正在加载中。


<div id="parent" class="anchor"></div>
## module.parent

- {Object} Module对象

在模块中会最先引入的模块。


<div id="children" class="anchor"></div>
## module.children

- {Array}

系统会按照该数组的顺序引入模块。