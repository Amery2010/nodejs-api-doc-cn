# 创建异步进程

* [child_process.exec(command[, options][, callback])](#exec)
* [child_process.execFile(file[, args][, options][, callback])](#execFile)
* [child_process.spawn(command[, args][, options])](#spawn)
  - [options.detached](#detached)
  - [options.stdio](#stdio)
* [child_process.fork(modulePath[, args][, options])](#fork)
* [在 Windows 上衍生 .bat 和 .cmd 文件](#spawning_bat_and_cmd_files_on_windows)

--------------------------------------------------


<div id="exec" class="anchor"></div>
## child_process.exec(command[, options][, callback])

- `command` {String} 要运行的命令，用空格分隔参数

+ `options` {Object}
  
  - `cwd` {String} 子进程的当前工作目录
  
  - `env` {Object} 环境变量键值对
  
  - `encoding` {String} （默认：'utf8'）
  
  - `shell` {String} 用于执行命令的 shell（默认：在 UNIX 上为 '/bin/sh'，在 Windows 上为 'cmd.exe'。该 shell 应该能够在 UNIX 上以 `-c` 启动或在 Windows 上以 `/s /c` 启动。在 Windows 中，命令行的解析应与 `cmd.exe` 兼容。）
  
  - `timeout` {Number} （默认：0）
  
  - `maxBuffer` {Number} 在 stdout 或 stderr 中所允许的最大数据量（以字节为单位）- 如果超过了该限制，子进程将被终止。（默认：`200*1024`）
  
  - `killSignal` {String} （默认：'SIGTERM'）
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
+ `callback` {Function} 在进程终止时与输出一起调用

  - `error` {Error}
  
  - `stdout` {String} | {Buffer}
  
  - `stderr` {String} | {Buffer}

- 返回：{ChildProcess}

衍生一个 shell，然后在该 shell 中执行该 `command`，缓冲任何产生的输出。

```javascript
const exec = require('child_process').exec;
const child = exec('cat *.js bad_file | wc -l', (error, stdout, stderr) => {
    console.log(`stdout: ${stdout}`);
    console.log(`stderr: ${stderr}`);
    if (error !== null) {
        console.log(`exec error: ${error}`);
    }
});
```

如果提供了一个 `callback` 函数，它以 `(error, stdout, stderr)` 的参数形式被调用。成功的话，`error` 会是 `null`。失败的话，`error` 会是一个 [Error](../errors/class_Error.md#) 实例。`error.code` 属性会是进程的退出码，而 `error.signal` 会被设置为终止进程的信号。除 `0` 以外的任何退出码都被认为是一个错误。

传给回调函数的 `stdout` 和 `stderr` 参数会包含子进程的 stdout 和 stderr 的输出结果。默认情况下，Node.js 会将输出解码为 UTF-8 并将结果字符串传递给回调函数。`encoding` 选项可用于指定用于解码 stdout 和 stderr 的字符编码。如果 `encoding` 是 `'buffer'`，`Buffer` 对象将代替传递给回调函数。

`options` 参数可以以第二个参数的形式传递，用于自定义如何衍生进程。默认的选项是：

```javascript
{
    encoding: 'utf8',
    timeout: 0,
    maxBuffer: 200 * 1024,
    killSignal: 'SIGTERM',
    cwd: null,
    env: null
}
```

如果 `timeout` 大于 `0` 并且子进程运行超过 `timeout` 毫秒，父进程将会发送由 `killSignal` 属性标识的信号（默认为 `'SIGTERM'`）。

*注意：不像 POSIX 系统调用中的 `exec()`，`child_process.exec()` 不会替换现有的进程和使用一个 shell 来执行命令。*