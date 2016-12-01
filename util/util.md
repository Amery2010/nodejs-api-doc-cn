# 方法和属性

* [util.isBoolean(object)](#isBoolean)
* [util.isNumber(object)](#isNumber)
* [util.isString(object)](#isString)
* [util.isObject(object)](#isObject)
* [util.isArray(object)](#isArray)
* [util.isFunction(object)](#isFunction)
* [util.isNull(object)](#isNull)
* [util.isUndefined(object)](#isUndefined)
* [util.isNullOrUndefined(object)](#isNullOrUndefined)
* [util.isSymbol(object)](#isSymbol)
* [util.isError(object)](#isError)
* [util.isRegExp(object)](#isRegExp)
* [util.isDate(object)](#isDate)
* [util.isBuffer(object)](#isBuffer)
* [util.isPrimitive(object)](#isPrimitive)
* [util.inspect(object[, options])](#inspect)
  - [定制 util.inspect 颜色](#customizing_inspect_colors)
  - [在对象上定制 inspect() 函数](#custom_inspect_function_on_Objects)
* [util.format(format[, ...])](#format)
* [util.inherits(constructor, superConstructor)](#inherits)
* [util.log(string)](#log)
* [util.error([...])](#error)
* [util.debug(string)](#debug)
* [util.debuglog(section)](#debuglog)
* [util.print([...])](#print)
* [util.puts([...])](#puts)
* [util.pump(readableStream, writableStream[, callback])](#pump)
* [util.deprecate(function, string)](#deprecate)

--------------------------------------------------


<div id="isBoolean" class="anchor"></div>
## util.isBoolean(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `Boolean`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isBoolean(1)
  // false
util.isBoolean(0)
  // false
util.isBoolean(false)
  // true
```


<div id="isNumber" class="anchor"></div>
## util.isNumber(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `Number`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isNumber(false)
  // false
util.isNumber(Infinity)
  // true
util.isNumber(0)
  // true
util.isNumber(NaN)
  // true
```


<div id="isString" class="anchor"></div>
## util.isString(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `String`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isString('')
  // true
util.isString('foo')
  // true
util.isString(String('foo'))
  // true
util.isString(5)
  // false
```


<div id="isObject" class="anchor"></div>
## util.isObject(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是一个严格的 `Object` **并且**不是一个 `Function`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isObject(5)
  // false
util.isObject(null)
  // false
util.isObject({})
  // true
util.isObject(function(){})
  // false
```


<div id="isArray" class="anchor"></div>
## util.isArray(object)

> 稳定度：0 - 已废弃

[Array.isArray](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray) 的内部别名。

如果给定的 'object' 是 `Array`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isArray([])
  // true
util.isArray(new Array)
  // true
util.isArray({})
  // false
```


<div id="isFunction" class="anchor"></div>
## util.isFunction(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `Function`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

function Foo() {}
var Bar = function() {};

util.isFunction({})
  // false
util.isFunction(Foo)
  // true
util.isFunction(Bar)
  // true
```


<div id="isNull" class="anchor"></div>
## util.isNull(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是严格的 `null`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isNull(0)
  // false
util.isNull(undefined)
  // false
util.isNull(null)
  // true
```


<div id="isUndefined" class="anchor"></div>
## util.isUndefined(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `undefined`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

var foo;
util.isUndefined(5)
  // false
util.isUndefined(foo)
  // true
util.isUndefined(null)
  // false
```


<div id="isNullOrUndefined" class="anchor"></div>
## util.isNullOrUndefined(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `null` 或 `undefined`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isNullOrUndefined(0)
  // false
util.isNullOrUndefined(undefined)
  // true
util.isNullOrUndefined(null)
  // true
```


<div id="isSymbol" class="anchor"></div>
## util.isSymbol(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `Symbol`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isSymbol(5)
  // false
util.isSymbol('foo')
  // false
util.isSymbol(Symbol('foo'))
  // true
```


<div id="isError" class="anchor"></div>
## util.isError(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是一个 [Error](../errors/class_Error.md#)，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isError(new Error())
  // true
util.isError(new TypeError())
  // true
util.isError({ name: 'Error', message: 'an error occurred' })
  // false
```

注意，这个方法依赖于 `Object.prototype.toString()` 的行为。当 `object` 参数操作 `@@toStringTag` 时，它有可能会获得一个不正确的结果。

```javascript
// This example requires the `--harmony-tostring` flag
const util = require('util');
const obj = { name: 'Error', message: 'an error occurred' };

util.isError(obj);
  // false
obj[Symbol.toStringTag] = 'Error';
util.isError(obj);
  // true
```


<div id="isRegExp" class="anchor"></div>
## util.isRegExp(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `RegExp`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isRegExp(/some regexp/)
  // true
util.isRegExp(new RegExp('another regexp'))
  // true
util.isRegExp({})
  // false
```


<div id="isDate" class="anchor"></div>
## util.isDate(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是 `Date`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isDate(new Date())
  // true
util.isDate(Date())
  // false (without 'new' returns a String)
util.isDate({})
  // false
```


<div id="isBuffer" class="anchor"></div>
## util.isBuffer(object)

> 稳定度：0 - 已废弃：请使用 [Buffer.isBuffer()](../buffer/class_Buffer.md#Buffer_isBuffer) 代替。

如果给定的 'object' 是 `Buffer`，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isBuffer({ length: 0 })
  // false
util.isBuffer([])
  // false
util.isBuffer(new Buffer('hello world'))
  // true
```


<div id="isPrimitive" class="anchor"></div>
## util.isPrimitive(object)

> 稳定度：0 - 已废弃

如果给定的 'object' 是一个基本类型，返回 `true`。否则，返回 `false`。

```javascript
const util = require('util');

util.isPrimitive(5)
  // true
util.isPrimitive('foo')
  // true
util.isPrimitive(false)
  // true
util.isPrimitive(null)
  // true
util.isPrimitive(undefined)
  // true
util.isPrimitive({})
  // false
util.isPrimitive(function() {})
  // false
util.isPrimitive(/^$/)
  // false
util.isPrimitive(new Date())
  // false
```


<div id="inspect" class="anchor"></div>
## util.inspect(object[, options])

返回 `object` 的字符串表示，这对调试有用。

可通过可选选项对象改变格式化字符串的某些方面：

* `showHidden` - 如果 `true`，那么对象的不可枚举和 symbol 属性也将被显示。默认为 `false`。

* `depth` - 告诉 `inspect` 在格式化对象时需要递归多少次。这对遍历大型且复杂的对象很有用。默认为 `2`。为了使其无限递归需要传 `null`。

* `colors` - 如果 `true`，那么输出将使用 ANSI 颜色代码样式。默认为 `false`。颜色可以定做，请参阅[定制 util.inspect 颜色](#customizing_inspect_colors)。

* `customInspect` - 如果 `false`，那么自定义在对象上的 `inspect(depth, opts)` 函数被检查时不会被调用。默认为 `true`。

检查 `util` 对象所有属性的例子：

```javascript
const util = require('util');

console.log(util.inspect(util, { showHidden: true, depth: null }));
```

当他们收到当前所谓的深度递归检查时，和传给 `util.inspect()` 一样的选项对象值会提供给他们自己定制的 `inspect(depth, opts)` 函数。


<div id="customizing_inspect_colors" class="anchor"></div>
### 定制 util.inspect 颜色

通过 `util.inspect.styles` 和 `util.inspect.colors` 对象来全局定制 `util.inspect` 的颜色输出（如果启用）。

`util.inspect.styles` 是一个分配每种风格一个从 `util.inspect.colors` 获得的颜色的映射。高亮显示样式和它们的默认值是：`number`（*黄色*）`boolean`（黄色）`string`（*绿色*）`date`（品红）`regexp`（*红色*）`null`（粗体）`undefined`（*灰色*）`special` - 此时唯一函数（青色）* `name`（故意没有样式）

预定义的颜色代码是：`white`、`grey`、`black`、`blue`、`cyan`、`green`、`magenta`、`red` 和 `yellow`。同样有 `bold`、`italic`、`underline` 和 `inverse` 代码。


<div id="custom_inspect_function_on_Objects" class="anchor"></div>
### 在对象上定制 inspect() 函数

对象也可以定义自己的 `inspect(depth)` 函数，`util.inspect()` 将调用和使用检查对象时的结果：

```javascript
const util = require('util');

var obj = {
    name: 'nate'
};
obj.inspect = function (depth) {
    return `{${this.name}}`;
};

util.inspect(obj);
// "{nate}"
```

你也完全可以返回另一个对象，并且返回的字符串将根据返回对象进行格式化。这类似于 `JSON.stringify()` 的工作机制：

```javascript
var obj = {
    foo: 'this will not show up in the inspect() output'
};
obj.inspect = function (depth) {
    return {
        bar: 'baz'
    };
};

util.inspect(obj);
// "{ bar: 'baz' }"
```


<div id="format" class="anchor"></div>
## util.format(format[, ...])

返回使用第一个参数作为一个类 `printf` 格式的格式化的字符串。

第一个参数是包含零个或多个*占位符*的字符串。每个占位符替换其对应参数的转换值。支持的占位符：

* `%s` - 字符串。

* `%d` - 数值（包括整数和浮点数）。

* `%j` - JSON。如果参数包含循环引用，则将其替换为字符串 `'[Circular]'`。

* `%%` - 百分号（`'%'`）。这不消耗参数。

如果占位符没有相应的参数，占位符不被替换。

```javascript
util.format('%s:%s', 'foo'); // 'foo:%s'
```

如果有比占位符更多的参数，额外的参数会被强制为字符串（对于对象和 symbol，使用 `util.inspect()`），然后拼接，用空格分隔。

```javascript
util.format('%s:%s', 'foo', 'bar', 'baz'); // 'foo:bar baz'
```

如果第一个参数是不是一个格式化的字符串，那么 `util.format()` 返回一个所有参数用空格分隔并连在一起的字符串。每个参数 `util.inspect()` 转换为字符串。

```javascript
util.format(1, 2, 3); // '1 2 3'
```


<div id="inherits" class="anchor"></div>
## util.inherits(constructor, superConstructor)

从一个[构造函数](https://developer.mozilla.org/zh-CN/JavaScript/Reference/Global_Objects/Object/constructor)中继承一个原型方法到另一个。`constructor` 的属性会被设置到从 `superConstructor` 创建的新对象上。

另外的好处是，`superConstructor` 将可以通过 `constructor.super_` 属性访问。

```javascript
const util = require('util');
const EventEmitter = require('events');

function MyStream() {
    EventEmitter.call(this);
}

util.inherits(MyStream, EventEmitter);

MyStream.prototype.write = function (data) {
    this.emit('data', data);
}

var stream = new MyStream();

console.log(stream instanceof EventEmitter); // true
console.log(MyStream.super_ === EventEmitter); // true

stream.on('data', (data) => {
    console.log(`Received data: "${data}"`);
})
stream.write('It works!'); // Received data: "It works!"
```


<div id="log" class="anchor"></div>
## util.log(string)

输出带时间戳的 `stdout`。

```javascript
require('util').log('Timestamped message.');
```


<div id="error" class="anchor"></div>
## util.error([...])

> 稳定度：0 - 已废弃：使用 [console.error()](../console/class_Console.md#consoleerrordata-args) 代替。

`console.error` 的过时前身。


<div id="debug" class="anchor"></div>
## util.debug(string)

> 稳定度：0 - 已废弃：使用 [console.error()](../console/class_Console.md#consoleerrordata-args) 代替。

`console.error` 的过时前身。


<div id="debuglog" class="anchor"></div>
## util.debuglog(section)

* `section` {String} 进行调试的程序部分

* 返回：{Function} 日志函数

这是用来创建有条件地写入到一个基于 `NODE_DEBUG` 环境变量存在的 `stderr` 的函数。如果 `section` 名称出现在那个环境变量中，那么返回的函数将类似于 `console.error()`。如果没有，那么返回一个无操作（空）的函数。

例如：

```javascript
var debuglog = util.debuglog('foo');

var bar = 123;
debuglog('hello from foo [%d]', bar);
```

如果程序在环境中运行时带有 `NODE_DEBUG=foo`，那么会有这样的输出：

```
FOO 3245: hello from foo [123]
```

`3245` 是进程 id，如果不是带着环境变量设置一起运行，那么它不会打印出任何东西。

你可以用逗号分隔的多个 `NODE_DEBUG` 环境变量。例如，`NODE_DEBUG=fs,net,tls`。


<div id="print" class="anchor"></div>
## util.print([...])

> 稳定度：0 - 已废弃：使用 [console.log()](../console/class_Console.md#consolelogdata-args) 代替。

`console.log` 的过时前身。


<div id="puts" class="anchor"></div>
## util.puts([...])

> 稳定度：0 - 已废弃：使用 [console.log()](../console/class_Console.md#consolelogdata-args) 代替。

`console.log` 的过时前身。


<div id="pump" class="anchor"></div>
## util.pump(readableStream, writableStream[, callback])

> 稳定度：0 - 已废弃：使用 readableStream.pipe(writableStream) 代替。

`stream.pipe()` 的过时前身。


<div id="deprecate" class="anchor"></div>
## util.deprecate(function, string)

标志着一个方法不应该再使用。

```javascript
const util = require('util');

exports.puts = util.deprecate(() => {
    for (var i = 0, len = arguments.length; i < len; ++i) {
        process.stdout.write(arguments[i] + '\n');
    }
}, 'util.puts: Use console.log instead');
```

它返回一个修正函数，在默认情况下警告一次。

如果设置了 `--no-deprecation`，那么这是一个无操作（空）的函数。在运行时通过 `process.noDeprecation` 的 boolean 进行配置（仅在加载模块之前设定时有效）。

如果设置了 `--trace-deprecation`，在第一次使用已过时的 API 时会将警告和堆栈跟踪记录到控制台中。在运行时通过 `process.traceDeprecation` 的 boolean 进行配置。

如果设置了 `--throw-deprecation`，那么当使用已过时的 API 时，应用程序会抛出一个异常。在运行时通过 `process.throwDeprecation` 的 boolean 进行配置。

`process.throwDeprecation` 优先于 `process.traceDeprecation`。