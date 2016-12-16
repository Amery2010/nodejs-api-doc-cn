# 文件模块

如果按确切的文件名没有查找到该模块，那么 Node.js 会尝试添加 `.js` 和 `.json` 拓展名进行查找，如果还未找到，最后会尝试添加 `.node` 的拓展名进行查找。

`.js` 文件会被解析为 JavaScript 文本文件，`.json` 文件会被解析为 JSON 文本文件。`.node` 文件会被解析为通过 `dlopen` 加载的编译后的插件模块。

请求的模块以 `'/'` 为前缀，则表示绝对路径。例如，`require('/home/marco/foo.js')` 将会加载的是 `/home/marco/foo.js` 文件。

请求的模块以 `'./'` 为前缀，则表示相对于调用 `require()` 的文件的路径。也就是说，`circle.js` 必须和 `foo.js` 在同一目录下以便于 `require('./circle')` 找到它。

当没有以 `'/'`、`'./'` 或 `'../'` 开头来表示文件时，这个模块必须是“核心模块”或加载自 `node_modules` 目录。

如果给定的路径不存在，`require()` 会抛出一个 `code` 属性为 `'MODULE_NOT_FOUND'` 的[错误](../errors/class_Error.md#)。