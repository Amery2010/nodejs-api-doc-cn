# 终端(TTY)

> 稳定度：2 - 稳定

`tty` 提供了 `tty.ReadStream` 和 `tty.WriteStream` 类。在大多数情况下，你不需要直接使用此模块。

当 Node.js 检测到它正在 TTY 上下文中运行时，那么 `process.stdin` 会是一个 `tty.ReadStream` 实例并且 `process.stdout` 会是一个 `tty.WriteStream` 实例。检查 Node.js 是否正在 TTY 上下文中运行的首选方法是去检测 `process.stdout.isTTY`：

``` bash
$ node -p -e "Boolean(process.stdout.isTTY)"
true
$ node -p -e "Boolean(process.stdout.isTTY)" | cat
false
```