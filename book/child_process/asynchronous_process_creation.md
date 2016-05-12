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

2. `'ipc'` - 创建一个父进程和子进程之间的 IPC 信道用以传递消息和文件描述符。一个 Child_Process 可能最多只能有*一个* IPC 标准输入输出文件描述符。设置该选项可启用 `ChildProcess.send()` 方法。如果子进程写了 JSON 信息到此文件描述符，`ChildProcess.on('message')` 事件处理器会被父进程触发。如果子进程是一个 Node.js 进程，一个已存在的 IPC 信道将可以在子进程中使用 `process.send()`、 `process.disconnect()`、 `process.on('disconnect')` 和 `process.on('message')`。

3. `'ignore'` - 指示 Node.js 在子进程中忽略 fd 。由于 Node.js 总是会为它衍生的进程打开 fds 0-2 ，将  fd 设置为 `'ignore'` 会引起 Node.js 打开 `/dev/null` 并将它附加到子进程的 fd 上。

4. `Stream` 对象 - 共享一个指向子进程的 tty、文件、socket 或管道的可读或可写流。流的底层文件描述符在子进程中重复着到对应该 `stdio` 数组的索引的 fd 。需要注意的是，该流必须要有一个底层描述符（文件流在发生 `'open'` 事件前不需要）。

5. 正整数 - 整数值被解释为正在父进程中打开的文件描述符。它和子进程共享，类似于 `Stream` 是如何被共享的。

6. `null`、`undefined` - 使用默认值。对于 stdio fds 0、1 和 2 （换句话说，stdin、stdout 和 stderr）而言是创建了一个管道。对于 fd 3 及以上而言，它的默认值为 `'ignore'`。

例子：

```javascript
const spawn = require('child_process').spawn;

// Child will use parent's stdios
spawn('prg', [], { stdio: 'inherit' });

// Spawn child sharing only stderr
spawn('prg', [], { stdio: ['pipe', 'pipe', process.stderr] });

// Open an extra fd=4, to interact with programs presenting a
// startd-style interface.
spawn('prg', [], { stdio: ['pipe', null, null, null, 'pipe'] });
```

*值得注意的是，当在父进程和子进程之间建立了一个 IPC 信道，并且子进程是一个 Node.js 进程，子进程的启动未引用（使用 `unref()`）该 IPC 信道，直到子进程为 `process.on('disconnected')` 事件注册了一个事件处理器。这运行子进程在没有进程的情况下正常退出通过开放的 IPC 信道保持开放状态。*

也可以看看：[child_process.exec()](#exec) 和 [child_process.fork()](#fork) 。


<div id="detached" class="anchor"></div>
#### options.detached

在 Windows 上，将 `options.detached` 设置为 `true`，使得子进程在父进程退出后继续运行成为可能。子进程将拥有自己的控制台窗口。*一旦启用一个子进程，它将不能被禁用。*

在非 Windows 平台上，如果将 `options.detached` 设置为 `true`，那么子进程将成为新的进程组和会话的领导者。请注意，子进程在父进程退出后可以继续运行，不管它们是否被分离。更多信息详见 `setsid(2)`。

默认情况下，父进程会等子进程被分离后才退出。为了防止父进程等待给定的 `child`，请使用 `child.unref()` 方法。这样做会导致父进程的事件循环不包括子进程的引用计数，这将允许父进程独立子进程退出，除非子进程和父进程之间建立了一个 IPC 信道。

当使用 `detached` 选项来启动一个长期运行的进程时，该进程不会在父进程退出后保持后台运行状态，除非它提供了一个不连接到父进程的 `stdio` 配置。如果该父进程的 `stdio` 被继承时，子进程会保持连接到控制终端。

一个长期运行的进程的例子，为了无视父母的终止，通过分离并且同时忽略其父进程的 `stdio` 文件描述符来实现：

```javascript
const spawn = require('child_process').spawn;

const child = spawn(process.argv[0], ['child_program.js'], {
    detached: true,
    stdio: ['ignore']
});

child.unref();
```

另外，你可以将子进程的输出重定向到文件：

```javascript
const fs = require('fs');
const spawn = require('child_process').spawn;
const out = fs.openSync('./out.log', 'a');
const err = fs.openSync('./out.log', 'a');

const child = spawn('prg', [], {
    detached: true,
    stdio: ['ignore', out, err]
});

child.unref();
```


<div id="fork" class="anchor"></div>
## child_process.fork(modulePath[, args][, options])

- `modulePath` {String} 在子进程中运行的模块

- `args` {Array} 字符串参数列表

+ `options` {Object}

  - `cwd` {String} 子进程的当前工作目录
  
  - `env` {Object} 环境变量键值对
  
  - `execPath` {String} 用来创建子进程的可执行文件
  
  - `execArgv` {Array} 传递给可执行文件的字符串参数列表（默认：`process.execArgv`）
  
  - `silent` {Boolean} 如果为 `true`，子进程中的 stdin、 stdout 和 stderr 会被导流到父进程中，否则它们会继承自父进程，详见 [child_process.spawn()](#spawn) 的 [stdio](#stdio) 中的 `'pipe'` 和 `'inherit'` 选项了解更多信息（默认是 `false`）
  
  - `uid` {Number} 设置该进程的用户标识。（详见 [setuid(2)](http://man7.org/linux/man-pages/man2/setuid.2.html)）
  
  - `gid` {Number} 设置该进程的组标识。（详见 [setgid(2)](http://man7.org/linux/man-pages/man2/setgid.2.html)）
  
- 返回：{ChildProcess}

`child_process.fork()` 方法是 [child_process.spawn()](#spawn) 的一种特殊情况，它被专门用于衍生新的 Node.js 进程。像 `child_process.spawn()` 一样，返回一个 `ChildProcess` 对象。返回的 `ChildProcess` 会有一个额外的内置的通信通道，它允许消息在父进程和子进程之间来回传递。查看 [ChildProcess#send()](./class_ChildProcess.md#send) 了解跟多细节。

牢记衍生的 Node.js 子进程与两者之间建立的 IPC 通信信道的异常是独立于父进程的，这一点非常重要。每个进程都有它自己的进程，使用自己的 V8 实例。由于需要额外的资源分配，因此不推荐衍生一大批 Node.js 子进程。

默认情况下，`child_process.fork() ` 会使用父进程中的 `process.execPath` 衍生新的 Node.js 实例。在 `options` 对象中的 `execPath` 属性允许替代要使用的执行路径。

Node.js 进程使用自定义的 `execPath` 启动会使用子进程的环境变量 `NODE_CHANNEL_FD` 中确定的文件描述符（fd）与父进程通信。 fd 上的输入和输出预计被分割成一行一行的 JSON 对象。

注意，不像 POSIX 系统回调中的 `fork()`，[child_process.fork()](#fork) 不会克隆当前进程。


<div id="spawning_bat_and_cmd_files_on_windows" class="anchor"></div>
## 在 Windows 上衍生 .bat 和 .cmd 文件

`child_process.exec()` 和 `child_process.execFile()` 之间最重要的区别是可以基于平台。在类 Unix 的操作平台（Unix、 Linux、 OSX）上，`child_process.execFile()` 更有效，因为它不需要衍生一个 shell。在 Windows 上，尽管 `.bat` 和 `.cmd` 文件在没有终端的情况下，它们自己不可自执行，因此不能使用 `child_process.execFile()` 启动。当在 Windows 下运行时，通过使用设置的 shell 选项的 `child_process.spawn()`，`child_process.exec()` 或通过将 `.bat` 和 `.cmd` 文件作为一个参数传给 `cmd.exe` 进行衍生（`shell` 选项和 `child_process.exec()` 所做的工作），这些操作可以调用 `.bat` 和 `.cmd` 文件。

```javascript
// On Windows Only ...
const spawn = require('child_process').spawn;
const bat = spawn('cmd.exe', ['/c', 'my.bat']);

bat.stdout.on('data', (data) => {
    console.log(data);
});

bat.stderr.on('data', (data) => {
    console.log(data);
});

bat.on('exit', (code) => {
    console.log(`Child exited with code ${code}`);
});

// OR...
const exec = require('child_process').exec;
exec('my.bat', (err, stdout, stderr) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(stdout);
});
```