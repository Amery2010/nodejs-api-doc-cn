# 创建同步进程

* [child_process.execSync(command[, options])](execSync)
* [child_process.execFileSync(file[, args][, options])](#execFileSync)
* [child_process.spawnSync(command[, args][, options])](#spawnSync)

--------------------------------------------------


<div id="execSync" class="anchor"></div>
## child_process.execSync(command[, options])

- `command` {String} 要运行的命令

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
  
