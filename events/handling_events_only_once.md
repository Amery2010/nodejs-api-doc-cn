# 绑定一次性事件

当使用 `eventEmitter.on()` 方法注册监听器时，这个监听器会在*每次*发出该命名事件时被调用。

``` javascript
const myEmitter = new MyEmitter();
let m = 0;
myEmitter.on('event', () => {
	console.log(++m);
});
myEmitter.emit('event');
// 打印：1
myEmitter.emit('event');
// 打印：2
```

当使用 `eventEmitter.once()` 方法时，可以注册对于特定事件最多调用一次的监听器。一旦触发了该事件，监听器就会被注销，随后调用该事件。

``` javascript
const myEmitter = new MyEmitter();
let m = 0;
myEmitter.once('event', () => {
	console.log(++m);
});
myEmitter.emit('event');
// 打印：1
myEmitter.emit('event');
// 忽略
```