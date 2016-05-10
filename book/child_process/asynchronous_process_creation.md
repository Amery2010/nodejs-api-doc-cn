# 创建异步进程

* [child_process.exec(command[, options][, callback])](#exec)
* [child_process.execFile(file[, args][, options][, callback])](#execFile)
* [child_process.spawn(command[, args][, options])](#spawn)
  - [options.stdio](#stdio)
  - [options.detached](#detached)
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


<div id="execFile" class="anchor"></div>
## child_process.execFile(file[, args][, options][, callback])

- `file` {String} 需要运行的可执行文件的名称或路径

- `args` {Array} 字符串参数列表

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  - `env` {Object} 环境变量键值对
  
  - `encoding` {String} （默认：'utf8'）
  
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

`child_process.execFile()` 函数类似于 [child_process.exec()](#exec)，它们的不同之处在于，前者不需要衍生一个 shell。而被指定的可执行文件直接衍生为一个新进程，也使得它比 [child_process.exec()](#exec) 更有效一些。

它支持和 `child_process.exec()` 一样的选项。由于没有衍生 shell，因此不支持像 I/O 重定向和文件查找这样的行为。

```javascript
const execFile = require('child_process').execFile;
const child = execFile('node', ['--version'], (error, stdout, stderr) => {
    if (error) {
        throw error;
    }
    console.log(stdout);
});
```

传给回调函数的 `stdout` 和 `stderr` 参数会包含子进程的 stdout 和 stderr 的输出结果。默认情况下，Node.js 会将输出解码为 UTF-8 并将结果字符串传递给回调函数。`encoding` 选项可用于指定用于解码 stdout 和 stderr 的字符编码。如果 `encoding` 是 `'buffer'`，`Buffer` 对象将代替传递给回调函数。


<div id="spawn" class="anchor"></div>
## child_process.spawn(command[, args][, options])

- `command` {String} 需要运行的命令

- `args` {Array} 字符串参数列表

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  - `env` {Object} 环境变量键值对
  
  - `stdio` {Array} | {String} 子进程的工作配置。（详见 [options.stdio](#stdio)）
  
  - `detached` {Boolean} 准备将子进程独立于父进程运行。它的具体行为取决于平台，详见（[options.detached](#detached)）
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
  - `shell` {Boolean} | {String} 如果为 `true`，在一个 shell 中运行 `command`。在 UNIX 上运行 '/bin/sh'，在 Windows 上运行 'cmd.exe'。一个不同的 shell 可以被指定为字符串。该 shell 应该能够在 UNIX 上以 `-c` 启动或在 Windows 上以 `/s /c` 启动。默认为 `false`（没有 shell）。
  
- 返回：{ChildProcess}

`child_process.spawn()` 方法使用给定的 `command` 带着 `args` 中的命令行参数来衍生一个新进程。如果省略，`args` 默认为一个空数组。

第三个参数可以用来指定其他选项，这些的默认值如下：

```javascript
{
    cwd: undefined,
    env: process.env
}
```

使用 `cwd` 来指定衍生进程的工作目录。如果没有给出，它默认是继承当前的工作目录。

使用 `env` 来指定环境变量，这将在新进程中可见，它默认为 `process.env`。

运行 `ls -lh /usr` 的例子，捕捉它的 `stdout`、 `stderr` 以及退出码：

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

例子：一种执行 `'ps ax | grep ssh'` 的复杂方式

```javascript
const spawn = require('child_process').spawn;
const ps = spawn('ps', ['ax']);
const grep = spawn('grep', ['ssh']);

ps.stdout.on('data', (data) => {
    grep.stdin.write(data);
});

ps.stderr.on('data', (data) => {
    console.log(`ps stderr: ${data}`);
});

ps.on('close', (code) => {
    if (code !== 0) {
        console.log(`ps process exited with code ${code}`);
    }
    grep.stdin.end();
});

grep.stdout.on('data', (data) => {
    console.log(`${data}`);
});

grep.stderr.on('data', (data) => {
    console.log(`grep stderr: ${data}`);
});

grep.on('close', (code) => {
    if (code !== 0) {
        console.log(`grep process exited with code ${code}`);
    }
});
```

检测执行失败的例子：

```javascript
const spawn = require('child_process').spawn;
const child = spawn('bad_command');

child.on('error', (err) => {
    console.log('Failed to start child process.');
});
```


<div id="stdio" class="anchor"></div>
#### options.stdio

`options.stdio` 选项用于配置子进程与父进程之间建立的管道。默认情况下，子进程的 stdin、 stdout 和 stderr 重定向到 `ChildProcess` 对象上相应的 `child.stdin`、 `child.stdout` 和 `child.stderr` 流。这等同于将 `options.stdio` 设置为 `['pipe', 'pipe', 'pipe']`。

为了方便起见，`options.stdio` 可以是下列字符串之一：

* `'pipe'` - 等同于 `['pipe', 'pipe', 'pipe']`（默认）

* `'ignore'` - 等同于 `['ignore', 'ignore', 'ignore']`

* `'inherit'` - 等同于 `[process.stdin, process.stdout, process.stderr]` 或 `[0,1,2]`

否则，`option.stdio` 的值是一个每个索引都对应子进程中的一个 fd 的数组。fd 中的 0 、 1 和 2 分别对应 stdin、 stdout 和 stderr。额外的 fd 可以被指定创建父进程和子进程之间的附加管道。该值是下列之一：

1. `'pipe'` - 创建一个子进程和父进程之间的管道。在管道的父端以 `ChildProcess.stdio[fd]` 的形式将父进程作为 `child_process` 对象上的一个属性暴露给外界。为 fd 创建的管道 0-2 分别替换为 `ChildProcess.stdin`、 `ChildProcess.stdout` 和 `ChildProcess.stderr` 也同样有效。

2. `'ipc'` 创建一个父进程和子进程之间的 IPC 通道用以传递消息和文件描述符。一个 Child_Process 可能最多只能有*一个* IPC 标准输入输出文件描述符。设置该选项可启用 `ChildProcess.send()` 方法。