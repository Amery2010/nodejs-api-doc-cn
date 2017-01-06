# 访问主模块

当 Node.js 直接运行一个文件时，`require.main` 就被设置为它的 `module`。这意味着你可以直接在测试中确定文件是否已运行。

``` javascript
require.main === module
```

对于 `foo.js` 文件而言，通过 `node foo.js` 运行则为 `true`；通过 `require('./foo')` 运行则为 `false`。

因为 `module` 提供了一个 `filename` 属性（通常等于 `__filename`），所以可以通过 `require.main.filename` 来获取当前应用程序的入口点。