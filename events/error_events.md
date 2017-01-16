# 错误事件

当 `EventEmitter` 实例中发生错误时，典型的行为就是触发一个 `'error'` 事件。这些在 Node.js 中被视为特殊情况。

如果 `EventEmitter` 实例没有注册过至少一个监听器，当一个 `'error'` 事件触发时，将抛出这个错误，打印堆栈跟踪，并退出 Node.js 进程。

``` javascript
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));
// Node.js 抛出错误，随后崩溃
```

为了防止 Node.js 进程崩溃，可以在[进程对象 uncaughtException 事件](../process/process.md#event_uncaughtException)上注册监听器或使用[域（domain）](../domain/)模块（*请注意，`domain` 模块已被弃用*）。

``` javascript
const myEmitter = new MyEmitter();

process.on('uncaughtException', (err) => {
	console.log('哇哦！这儿有个错误');
});

myEmitter.emit('error', new Error('whoops!'));
// 打印：哇哦！这儿有个错误
```

作为最佳实践，应该始终为 `'error'` 事件注册监听器：

``` javascript
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
	console.log('哇哦！这儿有个错误');
});
myEmitter.emit('error', new Error('whoops!'));
// 打印：哇哦！这儿有个错误
```