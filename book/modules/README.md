# 模块(Modules)

> 稳定度：3 - 已锁定

Node.js 有一个简单的模块加载系统。在 Node.js 中，文件和模块是一一对应的。以下例子是，`foo.js` 加载在同一目录下的 `circle.js` 模块。

`foo.js` 的内容：

```javascript
const circle = require('./circle.js');
console.log( `The area of a circle of radius 4 is ${circle.area(4)}`);
```

`circle.js` 的内容:

```javascript
const PI = Math.PI;

exports.area = (r) => PI * r * r;

exports.circumference = (r) => 2 * PI * r;
```

`circle.js` 模块输出了 `area()` 和 `circumference()` 两个函数。为了将函数和对象添加进模块，你可以将它们添加到特殊的 `exports` 对象下即可。

模块内的本地变量是私有的，就仿佛该模块被一个函数所包含。本例中，变量 `PI` 就是 `circle.js` 私有的。

如果你希望将你的模块导出为一个函数（比如构造函数）或一次导出一个完整的对象而不是一次一个的输出属性，请将 `module.exports` 替代 `exports` 使用。

下面，我将在 `bar.js` 中使用 `square` 模块导出的构造函数。

```javascript
const square = require('./square.js');
var mySquare = square(2);
console.log(`The area of my square is ${mySquare.area()}`);
```

`square` 模块定义在 `square.js` 中：

```javascript
// 为了导出没被修改过的模块, 必须使用 module.exports
module.exports = (width) => {
    return {
        area: () => width * width
    };
}
```

模块系统的实现在 `require("module")` 中。