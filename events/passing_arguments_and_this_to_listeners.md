# 给监听器传参

`eventEmitter.emit()` 方法允许将任意参数传递给监听器函数。需要牢记的是，一个普通的监听器函数被 `EventEmitter` 调用时，标准的 `this` 关键词会被刻意得设置成指向附加到监听器上的这个 `EventEmitter` 实例的引用。

``` javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', function (a, b) {
	console.log(a, b, this);
	// 打印：
	//   a b MyEmitter {
	//     domain: null,
	//     _events: { event: [Function] },
	//     _eventsCount: 1,
	//     _maxListeners: undefined }
});
myEmitter.emit('event', 'a', 'b');
```

也可以使用 ES6 的箭头函数作为监听器。然而，当你这么做时，`this` 关键词将不再引用 `EventEmitter` 实例。

``` javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
	console.log(a, b, this);
	// 打印：a b {}
});
myEmitter.emit('event', 'a', 'b');
```