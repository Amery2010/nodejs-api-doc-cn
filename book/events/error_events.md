# 错误事件

当一个 `EventEmitter` 实例发生错误，一个典型的操作就是触发一个 `'error'` 事件。这些都被视为 Node.js 中的一个特例。

如果一个 `EventEmitter` 没有注册过至少一个监听器，当一个 `'error'` 事件触发时，将抛出这个错误，打印跟踪堆栈，并退出 Node.js 进程。

```javascript
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));
// Throws and crashes Node.js
```

为了防止 Node.js 进程崩溃，开发者可以通过注册一个 `process.on('uncaughtException')` 事件的监听器或使用[域名（domain）](../domain/)模块（注意，这个 `domain` 模块*已被弃用*）。

```javascript
const myEmitter = new MyEmitter();

process.on('uncaughtException', (err) => {
    console.log('whoops! there was an error');
});

myEmitter.emit('error', new Error('whoops!'));
// Prints: whoops! there was an error
```

作为最佳实践，开发者应该总是注册 `'error'` 事件的监听器：

```javascript
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
    console.log('whoops! there was an error');
});
myEmitter.emit('error', new Error('whoops!'));
// Prints: whoops! there was an error
```