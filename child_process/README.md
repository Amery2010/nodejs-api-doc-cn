# 子进程(Child Processes)

> 稳定度：2 - 稳定

子进程模块提供了衍生子进程的能力，这个能力和 [popen(3)](http://linux.die.net/man/3/popen) 方式上类似，但不完全相同。这种能力主要由 `child_process.spawn()` 函数提供：

```javascript
const spawn = require('child_process').spawn;
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
    console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
    console.log(`stderr: ${data}`);
});

ls.on('close', (code) => {
    console.log(`child process exited with code ${code}`);
});
```

默认情况下，在 Node.js 的父进程和衍生的子进程之间会建立 `stdin`、`stdout` 和 `stderr` 的管道。这也使得数据流可以以无阻塞的方式通过这些管道。*但是请注意，有些程序内部使用行缓冲（line-buffered）I/O。虽然这并不影响 Node.js，它可能意味着发送到子过程数据可能无法立即消费。*

`child_process.spawn()` 方法异步衍生子进程，不会阻塞 Node.js 的事件循环。`child_process.spawnSync()` 函数以同步的方式提供了同样的功能，它会阻塞事件循环，直到衍生的子进程退出或终止。

为了方便起见，`child_process` 模块提供了少有的同步和异步的替代品 [child_process.spawn()](./synchronous_process_creation.md#spawn) 和 [child_process.spawnSync()](./asynchronous_process_creation.md#spawnSync) 。*请注意，这些替代品是在 `child_process.spawn()` 或 `child_process.spawnSync()` 的基础上实现的。*

* `child_process.exec()`：衍生一个 shell 并在 shell 内部运行一个命令，当完成时，会向回调函数传递 `stdout` 和 `stderr`。

* `child_process.execFile()`：和 `child_process.exec()` 类似，除了它直接衍生命令，而不需要先衍生一个 shell。

* `child_process.fork()`：衍生一个新的 Node.js 进程，并且通过建立一个允许父进程和子进程之间相互发送信息的 IPC 通讯通道来调用指定的模块。

* `child_process.execSync()`：`child_process.exec()` 的一个同步版本，这*会*阻塞 Node.js 的事件循环。

* `child_process.execFileSync()`：`child_process.execFile()` 的一个同步版本，这*会*阻塞 Node.js 的事件循环。

对于某些使用情况，如自动化 shell 脚本，[同步版本](./synchronous_process_creation.md#)可能更方便。在多数情况下，同步的方法会显著地影响性能，因为它拖延了事件循环直到衍生进程完成。