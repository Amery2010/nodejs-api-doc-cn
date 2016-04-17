# 给监听器传参

`eventEmitter.emit()` 方法允许给监听器函数传递一个随意设置的参数。需要牢记的是，一个普通的监听器函数被称为 `EventEmitter` 时，标准的 `this` 关键词会被刻意得设置成这个 `EventEmitter` 实例的引用并附加到这个监听器上。

```javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', function (a, b) {
    console.log(a, b, this);
    // Prints:
    //   a b MyEmitter {
    //     domain: null,
    //     _events: { event: [Function] },
    //     _eventsCount: 1,
    //     _maxListeners: undefined }
});
myEmitter.emit('event', 'a', 'b');
```

也有可能使用 ES6 的箭头函数（Arrow Function）作为监听器。而然，当你这么做时，`this` 关键词将不再是这个 `EventEmitter` 实例的引用。

```javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
    console.log(a, b, this);
    // Prints: a b {}
});
myEmitter.emit('event', 'a', 'b');
```