# 将文件夹作为模块

可以把程序和库放到一个单独的文件夹里，并提供单一入口来指向它。有三种方法，使一个文件夹可以作为 `require()` 的参数来加载。

第一种方式是在文件夹的根目录创建一个叫做 `package.json` 的文件，它需要指定一个 `main` 模块。下面是一个 `package.json` 文件的示例：

```javascript
{
    "name": "some-library",
    "main": "./lib/some-library.js"
}
```

示例中这个文件，如果是放在 `./some-library` 目录下面，那么 `require('./some-library')` 就将会去加载 `./some-library/lib/some-library.js` 。

这就是 Node.js 处理 package.json 文件的方式。

注意：如果在 `package.json` 文件中没能找到特殊的 `"main"` 入口，Node.js 将无法解析该模块，并抛出一个找不到该模块的默认错误：

```javascript
Error: Cannot find module 'some-library'
```

如果目录里没有 package.json 这个文件，那么 Node.js 就会尝试去加载这个路径下的 `index.js` 或者 `index.node` 。例如，若上面例子中没有 package.json ，那么 `require('./some-library')` 就将尝试加载以下文件：

- `./some-library/index.js`

- `./some-library/index.node`