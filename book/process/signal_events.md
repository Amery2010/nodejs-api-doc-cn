# 信号事件

当进程接收到信号时触发。标准的 POSIX 信号名称（如，`SIGINT`、 `SIGHUP` 等）列表详见（[sigaction(2)](http://man7.org/linux/man-pages/man2/sigaction.2.html)）。

监听 `SIGINT` 的例子：

```javascript
// Start reading from stdin so we don't exit.
process.stdin.resume();

process.on('SIGINT', () => {
    console.log('Got SIGINT.  Press Control-D to exit.');
});
```

发送 `SIGINT` 信号最简单的方式是在大多数终端程序中使用 `Control-C`。

注意：

`SIGUSR1` 由 Node.js 保留，用以开启调试器。它也可以设置一个监听器但不会中断调试器的启动。

`SIGTERM` 和 `SIGINT` 是非 Windows 平台上在以代码 `128 + signal number` 退出前重置终端模式的默认处理信号。如果监听到其中一个信号被设置，它默认的行为都会被移除（Node.js 将不复存在）。

`SIGPIPE` 默认被忽略。它可以设置监听器。

`SIGHUP` 在 Windows 上，是在控制台窗口关闭时产生，在其他平台上也是在各种类似的条件下产生（详见，[signal(7)](http://man7.org/linux/man-pages/man7/signal.7.html)）。它可以设置监听器，然而 Node.js 约10秒后会被 Windows 无条件终止。在非 Windows 平台上，`SIGHUP` 的默认行为是终止 Node.js ，但一旦设置了监听器，它的默认行为会被移除。

`SIGTERM` 在 Windows 是不被支持，它可以被监听的。

`SIGINT` 在所有平台的终端上都支持，并且通常是由 `CTRL+C`（虽然这也许配置的）产生的。当启用了终端原始模式时，它不再产生。

`SIGBREAK` 在 Windows 平台上，当按下 `CTRL+BREAK` 时发出，在非 Windows 平台上，它是可以被设置的，但没有办法发送或生成它。

`SIGWINCH` 在控制台已经调整后发出。在 Windows 中，当光标被移动时，或者当一个可读的 tty 在原始模式下使用时，这仅会在写入到控制台时发生。

`SIGKILL` 不能设置监听器，它在所有平台上的 Node.js 中会无条件终止。

`SIGSTOP` 不能设置监听器。

请注意，Windows 不支持发送信号，但 Node.js 提供了一些仿真方法 `process.kill()` 和 `child_process.kill()` 。发送信号 `0` 可用于测试一个进程是否存在。发送 `SIGINT`、 `SIGTERM` 和  `SIGKILL` 会导致目标进程无条件终止。