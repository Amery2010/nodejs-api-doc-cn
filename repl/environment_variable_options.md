# 环境变量

内置 repl（通过运行 `node` 或 `node -i` 调用）可以通过以下环境变量进行控制：

* `NODE_REPL_HISTORY` - 当给出有效路径时，持久性的 REPL 历史将被保存到指定的文件而不是在用户的主目录中的 `.node_repl_history`。设置这个变量为 `""` 将禁用持久性的 REPL 历史记录。它会从值中删除空格。

* `NODE_REPL_HISTORY_SIZE` - 默认为 `1000`。如果历史可用，它用于控制保留多少行历史记录。必须为正数。

* `NODE_REPL_MODE` - 可能是 `sloppy`、`strict` 或 `magic` 中的任何值。默认为 `magic`，它将在严格模式下自动运行“strict mode only”语句。