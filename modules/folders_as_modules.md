# 将文件夹作为模块

可以把程序和库放到一个单独的文件夹里，并提供单一入口来指向它。有三种方式可以将文件夹传递给 `require()` 作为参数。

第一种方式是在文件夹的根目录下创建一个 `package.json` 的文件，它指定一个 `main` 模块。以下是一个 `package.json` 的文件示例：

``` javascript
{
    "name": "some-library",
    "main": "./lib/some-library.js"
}
```

如果这是在 `./some-library` 文件夹中，那么 `require('./some-library')` 将尝试加载 `./some-library/lib/some-library.js`。

这就是 Node.js 处理 `package.json` 文件的方式。

注意：如果 `package.json` 中 `"main"` 条目指定的文件丢失，Node.js 将无法解析该模块，并抛出一个找不到该模块的默认错误：

``` javascript
Error: Cannot find module 'some-library'
```

如果目录里没有 `package.json` 这个文件，那么 Node.js 就会尝试去加载这个目录下的 `index.js` 或 `index.node` 文件。例如，如果上面例子中没有 `package.json`，那么 `require('./some-library')` 就将尝试加载以下文件：

* `./some-library/index.js`

* `./some-library/index.node`