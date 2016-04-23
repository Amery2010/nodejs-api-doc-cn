# 方法和属性

#### 事件

* ['message' 事件](#event_message)
* ['exit' 事件](#event_exit)
* ['beforeExit' 事件](#event_beforeExit)
* ['rejectionHandled' 事件](#event_rejectionHandled)
* ['unhandledRejection' 事件](#event_unhandledRejection)
* ['uncaughtException' 事件](#event_uncaughtException)
  - [警告：请正确使用 'uncaughtException' 事件](#using_event_uncaughtException_correctly)

#### 属性

* [process.config](#config)
* [process.env](#env)
* [process.platform](#platform)
* [process.arch](#arch)
* [process.release](#release)
* [process.mainModule](#mainModule)
* [process.title](#title)
* [process.pid](#pid)
* [process.connected](#connected)
* [process.exitCode](#exitCode)
* [process.stdin](#stdin)
* [process.stdout](#stdout)
* [process.stderr](#stderr)
* [process.execPath](#execPath)
* [process.execArgv](#execArgv)
* [process.argv](#argv)
* [process.version](#version)
* [process.versions](#versions)

#### 方法

* [process.send(message[, sendHandle[, options]][, callback])](#send)
* [process.nextTick(callback[, arg][, ...])](#nextTick)
* [process.disconnect()](#disconnect)
* [process.exit([code])](#exit)
* [process.abort()](#abort)
* [process.kill(pid[, signal])](#kill)
* [process.setgid(id)](#setgid)
* [process.getgid()](#getgid)
* [process.setuid(id)](#setuid)
* [process.getuid()](#getuid)
* [process.setgroups(groups)](#setgroups)
* [process.getgroups()](#getgroups)
* [process.setegid(id)](#setegid)
* [process.getegid()](#getegid)
* [process.seteuid(id)](#seteuid)
* [process.geteuid()](#geteuid)
* [process.initgroups(user, extra_group)](#initgroups)
* [process.uptime()](#uptime)
* [process.hrtime()](#hrtime)
* [process.memoryUsage()](#memoryUsage)
* [process.umask([mask])](#umask)
* [process.cwd()](#cwd)
* [process.chdir(directory)](#chdir)

--------------------------------------------------


<div id="event_message" class="anchor"></div>
## 'message' 事件

- `message` {Object} 一个已解析的 JSON 对象或原始值

- `sendHandle` {Handle 对象} 一个 [net.Socket](../net/class_net_Socket.md#) 或 [net.Server](../net/class_net_Server.md#) 或 `undefined`。

通过 [ChildProcess.send()](../child_process/class_ChildProcess.md#) 发送的信息，需要在子进程对象中使用 `'message'` 事件获取。


<div id="event_exit" class="anchor"></div>
## 'exit' 事件

当进程即将退出时触发。在这一点上，没有办法防止事件循环的退出，一旦所有的 `'exit'` 监听器已经完成监听，运行的进程将会退出。因此，你**只能**在处理程序时执行**同步**操作。这有利于对模块的状态进行检查（如单元测试）。进程代码退出时的回调函数有一个参数。

该事件只有当 Node.js 明确的通关过 `process.exit()` 退出或事件循环隐式外泄时才被触发。

监听 `'exit'` 的示例：

```javascript
process.on('exit', (code) => {
    // do *NOT* do this
    setTimeout(() => {
        console.log('This will not run');
    }, 0);
    console.log('About to exit with code:', code);
});
```


<div id="event_beforeExit" class="anchor"></div>
## 'beforeExit' 事件

该事件在 Node.js 清空其事件循环并且没有安排其他事情的情况下触发。通常情况下，Node.js 在没有计划工作时退出，但 `'beforeExit'` 监听器会进行异步调用，从而导致 Node.js 继续运行。

`'beforeExit'` 在显式终止的情况下不会触发，比如 [process.exit()](#exit) 或未捕获的异常，并且不应该将其替代 `'exit'` 事件，除非你是为了安排更多的工作。


<div id="event_rejectionHandled" class="anchor"></div>
## 'rejectionHandled' 事件

每当 Promise 被拒绝时触发，并且在一个事件循环后会给它附加一个错误程序（比如 `.catch()`）。此事件触发时带有以下参数：

* `p` 代表在 `'unhandledRejection'` 事件触发前的那个 promise，但目前获得了一个拒绝程序。

对于 promise 链而言，目前没有一个总可以用于处理拒绝的顶级概念。作为一个在本质上是异步，一个 promise 拒绝将在之后得到处理 —— 可能远远超过了事件循环以后触发的 `'unhandledRejection'` 事件的花费。

说明这一点的另一种方式是，不像在同步代码中有一个不断增长的未处理的异常列表，promise 有着不断增长和收缩的未处理的拒绝列表。在同步代码中，`'uncaughtException'` 事件告诉你何时未处理的异常列表在不断增长。在异步代码中，`'unhandledRejection'` 事件告诉你何时未处理的拒绝列表在增长，`'rejectionHandled'` 事件告诉你何时未处理的拒绝列表在收缩。

例如使用拒绝检查钩子以便于在给给定时间内保持一份所有被拒绝的 promise 的理由的映射：

```javascript
const unhandledRejections = new Map();
process.on('unhandledRejection', (reason, p) => {
    unhandledRejections.set(p, reason);
});
process.on('rejectionHandled', (p) => {
    unhandledRejections.delete(p);
});
```

这份映射表会随时间的推移增长和收缩，反映了拒绝在何时未处理，何时被处理。你可以在一些错误日志中记录这些错误，无论是定期（尤其是针对长期运行的程序，在一个错误可能无限增长的的程序下，允许你清理该映射）还是在进程退出时（对脚本而言更方便）。


<div id="event_unhandledRejection" class="anchor"></div>
## 'unhandledRejection' 事件

每当 Promise 被拒绝时触发，没有错误处理程序附加到事件循环内的 promise 上。当程序设计中的 promise 异常被封装成拒绝 promise 时，这样的 promise 可以使用 [promise.catch(...)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) 捕捉和处理，并且拒绝可以通过 promise 链传播。该事件有利于检测和保持对还没被处理的那些被拒绝的 promise 的跟踪。该事件触发时带有以下参数：

* `reason` 包含被拒绝的 promise 的对象（通常是一个[错误](../errors/class_Error.md#)实例）。

* `p` 被拒绝的 promise。

这是一个将每一个未处理的拒绝记录到控制台的例子：

```javascript
process.on('unhandledRejection', (reason, p) => {
    console.log("Unhandled Rejection at: Promise ", p, " reason: ", reason);
    // application specific logging, throwing an error, or other logic here
});
```

例如，这是一个将会触发 `'unhandledRejection'` 事件的拒绝：

```javascript
somePromise.then((res) => {
    return reportToUser(JSON.pasre(res)); // note the typo (`pasre`)
}); // no `.catch` or `.then`
```

这是一个也会触发 `'unhandledRejection'` 事件的编码模式的例子：

```javascript
function SomeResource() {
    // Initially set the loaded status to a rejected promise
    this.loaded = Promise.reject(new Error('Resource not yet loaded!'));
}

var resource = new SomeResource();
// no .catch or .then on resource.loaded for at least a turn
```

在这种情况下，你可能不希望将跟踪拒绝作为一个开发者的错误，就像你对其他 `'unhandledRejection'` 事件那样。为了解决这个问题，你可以在 `resource.loaded` 上附加一个假的 `.catch(() => { })` 处理器，防止触发 `'unhandledRejection'` 事件，或者你也可以使用 ['rejectionHandled'](#event_rejectionHandled) 事件。


<div id="event_uncaughtException" class="anchor"></div>
## 'uncaughtException' 事件

当异常一路冒泡回事件循环时会触发 `'uncaughtException'` 事件。默认，Node.js 通过在 stderr 中打印堆栈跟踪并退出来处理这种情况。可以通过给 `'uncaughtException'` 事件添加处理器来覆盖这种默认行为。

例如：

```javascript
process.on('uncaughtException', (err) => {
    console.log(`Caught exception: ${err}`);
});

setTimeout(() => {
    console.log('This will still run.');
}, 500);

// Intentionally cause an exception, but don't catch it.
nonexistentFunc();
console.log('This will not run.');
```


<div id="using_event_uncaughtException_correctly" class="anchor"></div>
#### 警告：请正确使用 'uncaughtException' 事件

请注意，`'uncaughtException'` 是一种异常处理的粗机制，希望只作为最后的手段。该事件*不应该*被用做 `On Error Resume Next` 的替代品。未处理的异常本质上意味着应用程序处于不确定状态。试图恢复应用程序代码而不是从异常恢复正常可能会导致额外的不可预见的和不可预知的问题。

从事件处理程序中抛出的异常不会被捕获。相反，会以非零退出代码退出进程，并且打印堆栈跟踪。这是为了避免无限递归。

尝试从一个未捕获到异常后恢复正常类似于在升级电脑时拔出电源线——十次里的前九次时间是没有任何反应的——但在第十次的时候系统损坏了。

`'uncaughtException'` 的正确用法是在进程关闭前执行分配资源的同步清理（如，文件描述符、处理程序等）。在 `'uncaughtException'` 事件后恢复正常操作是不安全的。