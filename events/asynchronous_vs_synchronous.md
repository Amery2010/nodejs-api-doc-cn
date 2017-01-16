# 异步和同步

`EventListener` 会按照监听器的注册顺序同步地调用所有监听器。这对于确保事件的正确排序很重要以避免竞争条件或逻辑错误。在适当的时候，监听器函数也可以通过使用 `setImmediate()` 或 `process.nextTick()` 方法切换到异步操作模式：

``` javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
	setImmediate(() => {
		console.log('这是异步发生的');
	});
});
myEmitter.emit('event', 'a', 'b');
```