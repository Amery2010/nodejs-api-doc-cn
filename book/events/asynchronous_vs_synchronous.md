# 异步vs同步

`EventListener` 会按照监听器的注册顺序同步调用它的所有的监听器。这在确保事件的正确时序上非常重要，也能避免竞争条件或逻辑错误。在适当的时候，监听器函数也可以通过使用 `setImmediate()` 和 `process.nextTick()` 方法去选择一种异步的操作模式：

```javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
    setImmediate(() => {
        console.log('this happens asynchronously');
    });
});
myEmitter.emit('event', 'a', 'b');
```