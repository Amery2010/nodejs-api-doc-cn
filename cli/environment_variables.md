# 环境变量

* [NODE_DEBUG=module[,…]](#nodedebugmodule)
* [NODE_PATH=path[:…]](#nodepathpath)
* [NODE_DISABLE_COLORS=1](#nodedisablecolors1)
* [NODE_ICU_DATA=file](#nodeicudatafile)
* [NODE_REPL_HISTORY=file](#nodereplhistoryfile)

--------------------------------------------------


## NODE_DEBUG=module[,…]

`','` - 分隔的应该打印调试信息的核心模块列表。


## NODE_PATH=path[:…]

`':'` - 分隔的模块搜索路径的前缀列表。

*请注意，在 Windows 中，是用 `';'` - 分隔的列表。*


## NODE_DISABLE_COLORS=1

当设置为 `1` 时，将不会在 REPL 中使用颜色。


## NODE_ICU_DATA=file

ICU（Intl 对象）数据的数据路径。将在编译时使用 `small-icu` 支持的扩展链接的数据。


## NODE_REPL_HISTORY=file

用于存储持久性的 REPL 历史记录的文件的路径。它的默认路径是 `~/.node_repl_history`，该变量覆盖该值。将值设置为空字符串（`""` 或 `" "`）会禁用持久性的 REPL 历史记录。