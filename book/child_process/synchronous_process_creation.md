# 创建同步进程

* [child_process.execSync(command[, options])](execSync)
* [child_process.execFileSync(file[, args][, options])](#execFileSync)
* [child_process.spawnSync(command[, args][, options])](#spawnSync)

--------------------------------------------------


`child_process.spawnSync()`、`child_process.execSync()` 和 `child_process.execFileSync()` 方法是同步的并且**会**阻塞 Node.js 的事件循环，暂停任何额外代码的执行直到衍生的进程退出为止。

像这样的阻塞调用有利于简化通用脚本任务，并在启动时有利于简化应用配置的加载/处理。


<div id="execSync" class="anchor"></div>
## child_process.execSync(command[, options])

- `command` {String}

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  + `input` {String} | {Buffer} 该值会被作为传递到衍生进程中的标准输入
    
    - 提供的值将会覆盖 `stdio[0]`

  + `stdio` {Array} 子进程中的 stdio 配置（默认：`'pipe'`）。
    
    - `stderr`除非指定了 `stdio`，否则默认会输出到父进程中的 stderr。
    
  - `env` {Object} 环境变量键值对
  
  - `shell` {String} 用于执行命令的 shell（默认：在 UNIX 上为 '/bin/sh'，在 Windows 上为 'cmd.exe'。该 shell 应该能够在 UNIX 上以 `-c` 启动或在 Windows 上以 `/s /c` 启动。在 Windows 中，命令行的解析应与 `cmd.exe` 兼容。）
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
  - `timeout` {Number} 进程被运行运行的最大时间量，以毫秒为单位（默认：undefined）
  
  - `killSignal` {String} 当衍生进程将被杀死时要使用的信号值。（默认：'SIGTERM'）
  
  - `maxBuffer` {Number} 在 stdout 或 stderr 中所允许的最大数据量（以字节为单位）- 如果超过了该限制，子进程将被终止。
  
  - `encoding` {String} 用于所有 stdio 输入和输出的编码
  
- 返回：{Buffer} | {String} 该命令的标准输出
  
`child_process.execSync()` 基本等同于 `child_process.exec()`，除了该方法直到子进程完全关闭前不会返回结果。当遇到超时并发送了 `killSignal` 信号时，该方法直到进程完全退出前不会返回结果。*请注意，如果拦截了子进程并且处理了 `SIGTERM` 信号但没有退出，父进程会一直等待直到子进程退出为止。*

如果进程超时，或有一个非零退出码，该方法**会**抛出错误。该 [Error](../errors/class_Error.md#) 对象包含从 [child_process.spawnSync()](#spawnSync) 获取的整个结果（对象的 Error 属性）。


<div id="execFileSync" class="anchor"></div>
## child_process.execFileSync(file[, args][, options])

- `file` {String} 要运行的可执行文件的名称或路径

- `args` {Array} 字符串参数列表

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  + `input` {String} | {Buffer} 该值会被作为传递到衍生进程中的标准输入
    
    - 提供的值将会覆盖 `stdio[0]`

  + `stdio` {Array} 子进程中的 stdio 配置（默认：`'pipe'`）。
    
    - `stderr`除非指定了 `stdio`，否则默认会输出到父进程中的 stderr。
    
  - `env` {Object} 环境变量键值对
  
  - `shell` {String} 用于执行命令的 shell（默认：在 UNIX 上为 '/bin/sh'，在 Windows 上为 'cmd.exe'。该 shell 应该能够在 UNIX 上以 `-c` 启动或在 Windows 上以 `/s /c` 启动。在 Windows 中，命令行的解析应与 `cmd.exe` 兼容。）
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
  - `timeout` {Number} 进程被运行运行的最大时间量，以毫秒为单位（默认：undefined）
  
  - `killSignal` {String} 当衍生进程将被杀死时要使用的信号值。（默认：'SIGTERM'）
  
  - `maxBuffer` {Number} 在 stdout 或 stderr 中所允许的最大数据量（以字节为单位）- 如果超过了该限制，子进程将被终止。
  
  - `encoding` {String} 用于所有 stdio 输入和输出的编码
  
- 返回：{Buffer} | {String} 该命令的标准输出
  
`child_process.execFileSync()` 基本等同于 `child_process.execFile()`，除了该方法直到子进程完全关闭前不会返回结果。当遇到超时并发送了 `killSignal` 信号时，该方法直到进程完全退出前不会返回结果。*请注意，如果拦截了子进程并且处理了 `SIGTERM` 信号但没有退出，父进程会一直等待直到子进程退出为止。*

如果进程超时，或有一个非零退出码，该方法**会**抛出错误。该 [Error](../errors/class_Error.md#) 对象包含从 [child_process.spawnSync()](#spawnSync) 获取的整个结果（对象的 Error 属性）。


<div id="spawnSync" class="anchor"></div>
## child_process.spawnSync(command[, args][, options])

- `command` {String} 要运行的命令

- `args` {Array} 字符串参数列表

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  + `input` {String} | {Buffer} 该值会被作为传递到衍生进程中的标准输入
    
    - 提供的值将会覆盖 `stdio[0]`

  + `stdio` {Array} 子进程中的 stdio 配置。
    
  - `env` {Object} 环境变量键值对
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
  - `timeout` {Number} 进程被运行运行的最大时间量，以毫秒为单位（默认：undefined）
  
  - `killSignal` {String} 当衍生进程将被杀死时要使用的信号值。（默认：'SIGTERM'）
  
  - `maxBuffer` {Number} 在 stdout 或 stderr 中所允许的最大数据量（以字节为单位）- 如果超过了该限制，子进程将被终止。
  
  - `encoding` {String} 用于所有 stdio 输入和输出的编码。（默认：'buffer'）
  
  - `shell` {Boolean} | {String} 如果为 `true`，在一个 shell 中运行 `command`。在 UNIX 上运行 '/bin/sh'，在 Windows 上运行 'cmd.exe'。一个不同的 shell 可以被指定为字符串。该 shell 应该能够在 UNIX 上以 `-c` 启动或在 Windows 上以 `/s /c` 启动。默认为 `false`（没有 shell）。

+ 返回：{Object}

  - `pid` {Number} 子进程的 pid
  
  - `output` {Array} 从 stdio 输出的结果数组
  
  - `stdout` {Buffer} | {String} `output[1]` 的内容
  
  - `stderr` {Buffer} | {String} `output[2]` 的内容
  
  - `status` {Number} 子进程的退出码
  
  - `signal` {String} 用于杀死子进程的信号
  
  - `error` {Error} 如果子进程失败或超时产生的错误对象。

`child_process.spawnSync()` 基本等同于 `child_process.spawn()`，除了该方法直到子进程完全关闭前不会返回结果。当遇到超时并发送了 `killSignal` 信号时，该方法直到进程完全退出前不会返回结果。*请注意，如果拦截了子进程并且处理了 `SIGTERM` 信号但没有退出，父进程会一直等待直到子进程退出为止。*