# 模块(Modules)

* [方法和属性](./module.md)
* [核心模块](./core_modules.md)
* [文件模块](./file_modules.md)
* [模块包装器](./the_module_wrapper.md)
* [将文件夹作为模块](./folders_as_modules.md)
* [从 node_modules 文件夹加载模块](./loading_from_node_modules_folders.md)
* [从全局文件夹加载模块](./loading_from_the_global_folders.md)
* [循环](./cycles.md)
* [缓存](./caching.md)
  * [模块缓存的注意事项](./caching.md#模块缓存的注意事项)
* [访问主模块](./accessing_the_main_module.md)
* [总的来说...](./all_together.md)
* [附录：包管理器的技巧](./package_manager_tips.md)

--------------------------------------------------

> 稳定度：3 - 已锁定

Node.js 有一个简单的模块加载系统。在 Node.js 中，文件和模块是一一对应的（每个文件被视为一个单独的模块）。举个例子，`foo.js` 加载同一目录下的 `circle.js` 模块。

`foo.js` 的内容：

``` javascript
const circle = require('./circle.js');
console.log(`The area of a circle of radius 4 is ${circle.area(4)}`);
```

`circle.js` 的内容:

``` javascript
const PI = Math.PI;

exports.area = (r) => PI * r * r;

exports.circumference = (r) => 2 * PI * r;
```

`circle.js` 模块导出了 `area()` 和 `circumference()` 两个函数。为了将函数和对象添加进你的模块根，你可以将它们添加到特殊的 `exports` 对象下。

模块内的本地变量是私有的，因为模块被 Node.js 包装在一个函数中（详见[模块包装器](./the_module_wrapper.md)）。在这个例子中，变量 `PI` 就是 `circle.js` 私有的。

如果你希望将你的模块根导出为一个函数（比如构造函数）或一次导出一个完整的对象而不是每一次都创建一个属性，请赋值给 `module.exports` 而不是 `exports`。

下面，我将在 `bar.js` 中使用 `square` 模块导出的构造函数。

``` javascript
const square = require('./square.js');
var mySquare = square(2);
console.log(`The area of my square is ${mySquare.area()}`);
```

`square` 模块定义在 `square.js` 中：

``` javascript
// 赋值给 exports 将不会修改模块，必须使用 module.exports
module.exports = (width) => {
	return {
		area: () => width * width
	};
}
```

模块系统在 `require("module")` 中实现。