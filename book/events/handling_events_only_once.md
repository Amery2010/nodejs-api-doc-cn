# 绑定一次性事件

当使用 `eventEmitter.on()` 方法注册一个监听器，这个监听器会在指定事件每次触发时被调用。

```javascript
const myEmitter = new MyEmitter();
var m = 0;
myEmitter.on('event', () => {
    console.log(++m);
});
myEmitter.emit('event');
// Prints: 1
myEmitter.emit('event');
// Prints: 2
```

当使用 `eventEmitter.once()` 方法时，一个已注册的监听器在调用后可能被立即取消注册。

```javascript
const myEmitter = new MyEmitter();
var m = 0;
myEmitter.once('event', () => {
    console.log(++m);
});
myEmitter.emit('event');
// Prints: 1
myEmitter.emit('event');
// Ignored
```