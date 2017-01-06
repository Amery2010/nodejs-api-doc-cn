# 循环

当循环调用 `require()` 时，模块可能在未完成执行时被返回。

考虑这样一种情况:

`a.js`：

``` javascript
console.log('a starting');
exports.done = false;
const b = require('./b.js');
console.log('in a, b.done = %j', b.done);
exports.done = true;
console.log('a done');
```

`b.js`：

``` javascript
console.log('b starting');
exports.done = false;
const a = require('./a.js');
console.log('in b, a.done = %j', a.done);
exports.done = true;
console.log('b done');
```

`main.js`：

``` javascript
console.log('main starting');
const a = require('./a.js');
const b = require('./b.js');
console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
```

当 `main.js` 加载 `a.js` 时，`a.js` 反向加载 `b.js`。那时，`b.js` 会尝试去加载 `a.js`。为了防止无限的循环，`a.js` 会返回一个 `exports` 对象的 **未完成副本** 给 `b.js` 模块。之后 `b.js` 完成加载，并将 `exports` 对象返回给 `a.js` 模块。

当 `main.js` 加载这两个模块时，它们都已经完成加载了。因此，该程序的输出将会是：

``` shell
$ node main.js
main starting
a starting
b starting
in b, a.done = false
b done
in a, b.done = true
a done
in main, a.done=true, b.done=true
```

如果你的程序里有循环的依赖模块，请确保它们按计划执行。