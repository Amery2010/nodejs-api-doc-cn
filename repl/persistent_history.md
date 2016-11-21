# 永久历史

默认情况下，REPL 会保持 `node` REPL 会话之间的历史，通过保存到在用户的主目录中的 `.node_repl_history`。这可以通过设置环境变量 `NODE_REPL_HISTORY=""` 来禁用。

## NODE_REPL_HISTORY_FILE

> 稳定度：0 - 已废弃：请改用 NODE_REPL_HISTORY。

在 Node.js/io.js v2.x 之前的版本中，REPL 历史是通过使用 `NODE_REPL_HISTORY_FILE` 环境变量来控制的，并且历史记录以 JSON 格式保存。此变量现已被废弃，并且你的 REPL 历史记录将自动转换为使用纯文本。新文件将保存到你的主目录，或由 `NODE_REPL_HISTORY` 变量定义的目录，如[此处](./environment_variable_options.md#)所述。