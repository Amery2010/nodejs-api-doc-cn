# 循环

当循环调用 `require()` 时，一个模块可能在返回时并不被执行。

考虑这样一种情形:

`a.js`：

```javascript
console.log('a starting');
exports.done = false;
const b = require('./b.js');
console.log('in a, b.done = %j', b.done);
exports.done = true;
console.log('a done');
```

`b.js`：

```javascript
console.log('b starting');
exports.done = false;
const a = require('./a.js');
console.log('in b, a.done = %j', a.done);
exports.done = true;
console.log('b done');
```

`main.js`：

```javascript
console.log('main starting');
const a = require('./a.js');
const b = require('./b.js');
console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
```

首先 `main.js` 加载 `a.js` ,接着 `a.js` 又去加载 `b.js` 。这时，`b.js` 会尝试去加载 `a.js` 。为了防止无限的循环，`a.js` 会返回一个 **unfinished copy** 给 `b.js` 。然后 `b.js` 完成加载，并将其它的 `exports` 对象返回给 `a.js` 模块。

这样 `main.js` 就把这两个模块都加载完成了。这段程序的输出如下：

```
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

如果你的程序里有循环的依赖模块，请确保它们是按计划执行的。