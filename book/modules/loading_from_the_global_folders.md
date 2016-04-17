# 从全局文件夹加载模块

如果 `NODE_PATH` 环境变量设置为一个以冒号分割的绝对路径列表，在其他位置找不到模块时 Node.js 将会从这些路径中搜索。（注意：在 Windows 操作系统上，`NODE_PATH` 是以分号间隔的。）

`NODE_PATH` 最初创建用以支持从不同路径加载模块，它不会在当前[模块解析](./all_together.md#)算法运行前被使用。

虽然目前仍然支持 `NODE_PATH` ，但在 Node.js 生态系统约定依赖模块的存放路径后已经很少用到了。人们有时部署依赖 `NODE_PATH` 的模块，不知道必须设置 `NODE_PATH` 时会表露出惊讶表情。有时，一个模块的依赖关系发生变化，从而加载了从 `NODE_PATH` 搜索到模块，可能会产生一个不同版本（甚至不同的模块）。（这一段的[原文](https://nodejs.org/dist/latest-v5.x/docs/api/modules.html#modules_loading_from_the_global_folders)<del>实在太过意识流</del>太难理解，我只能根据自身理解进行翻译，如果有朋友愿意，请帮助我更新此处翻译，[译者](https://github.com/Amery2010)注）

此外，Node.js 将会搜索以下路径：

1. `$HOME/.node_modules`

2. `$HOME/.node_libraries`

3. `$PREFIX/lib/node`

其中 `$HOME` 是用户的主目录，`$PREFIX` 是 Node.js 里配置的 `node_prefix`。

这些大多是由于历史原因产生的。**强烈建议读者将所有的依赖模块放到 `node_modules` 文件夹里**，它们将更快更可靠地被加载。





