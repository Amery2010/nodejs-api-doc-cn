# 模块包装器

在执行模块代码之前，Node.js 会使用一个如下所示的函数包装器将其包装：

``` javascript
(function (exports, require, module, __filename, __dirname) {
// 你的模块代码实际上应该在这里
});
```

通过这样做，Node.js 实现了以下几点：

* 它保持顶级变量（用 `var`、`const` 或 `let` 定义）作用域是在模块内部而不是在全局对象上。

* 它有助于提供一些实际上特定于模块的全局变量，例如：

	- 实现者可以用 `module` 和 `exports` 对象从模块中导出值。
	
	- 快捷变量 `__filename` 和 `__dirname` 包含了模块的文件名和目录的绝对路径。