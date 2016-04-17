# 高级用法

启用和访问调试器的另一种方式是在启动 Node.js 时添加 `--debug` 命令行标志，或向已存在的 Node.js 进程发送 `SIGUSR1` 信号。

一个进程一旦以这种方式进入了调试模式，它就可以被 Node.js 调试器连接使用，通过连接已运行的进程的 `pid` 或访问这个正在监听的调试器的 URI：

* `node debug -p <pid>` - 通过 `pid` 连接进程

* `node debug <URI>` - 通过类似 localhost:5858 的 URI 连接进程