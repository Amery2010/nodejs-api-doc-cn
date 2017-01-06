# 从全局文件夹加载模块

如果 `NODE_PATH` 环境变量设置为一个以冒号分割的绝对路径列表。如果在其他位置找不到模块时，那么 Node.js 将会从这些路径中搜索。（注意：在 Windows 操作系统中，`NODE_PATH` 是以分号间隔的。）

`NODE_PATH` 最初创建用以支持从不同路径加载模块，它不会在当前[模块解析](./all_together.md#)算法运行前被使用。

虽然目前仍然支持 `NODE_PATH` ，但在 Node.js 生态系统约定依赖模块的存放路径后已经很少用到了。有时，部署依赖 `NODE_PATH` 的模块，会在别人不知道必须设置 `NODE_PATH` 的情况下出现异常行为。有时，模块的依赖关系会发生变化，导致在搜索 `NODE_PATH` 时加载了不同的版本（甚至不同的模块）。

此外，Node.js 还会搜索以下路径：

1. `$HOME/.node_modules`

2. `$HOME/.node_libraries`

3. `$PREFIX/lib/node`

其中 `$HOME` 是用户的主目录，`$PREFIX` 是 Node.js 里配置的 `node_prefix`。

这些大多是由于历史原因产生的。**强烈建议你们将所有的依赖模块放在 `node_modules` 文件夹中**。它们将会更快、更可靠地被加载。





