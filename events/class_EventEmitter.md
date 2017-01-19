# EventEmitter 类

* ['newListener' 事件](#newListener-事件)
* ['removeListener' 事件](#removeListener-事件)
* [EventEmitter.defaultMaxListeners](#eventemitterdefaultmaxlisteners)
* [EventEmitter.listenerCount(emitter, eventName)](#eventemitterlistenercountemitter-eventname) <del>已废弃</del>
* [emitter.on(eventName, listener)](#emitteroneventname-listener)
* [emitter.once(eventName, listener)](#emitteronceeventname-listener)
* [emitter.addListener(eventName, listener)](#emitteraddlistenereventname-listener)
* [emitter.prependListener(eventName, listener)](#emitterprependlistenereventname-listener)
* [emitter.prependOnceListener(eventName, listener)](#emitterprependoncelistenereventname-listener)
* [emitter.removeListener(eventName, listener)](#emitterremovelistenereventname-listener)
* [emitter.removeAllListeners([eventName])](#emitterremovealllistenerseventname)
* [emitter.emit(eventName[, ...args])](#emitteremiteventname-args)
* [emitter.listeners(eventName)](#emitterlistenerseventname)
* [emitter.listenerCount(eventName)](#emitterlistenercounteventname)
* [emitter.setMaxListeners(n)](#emittersetmaxlistenersn)
* [emitter.getMaxListeners()](#emittergetmaxlisteners)

--------------------------------------------------


添加：v0.1.26

`EventEmitter` 类由 `events` 模块定义和公开：

``` javascript
const EventEmitter = require('events');
```

所有的事件触发器都会在新的监听器被添加时触发 `'newListener'` 事件；在一个监听器被移除时触发 `'removeListener'` 事件。


## 'newListener' 事件

添加：v0.1.26

- `eventName` {String} | {Symbol} 要监听的事件名称

- `listener` {Function} 回调函数

`EventEmitter` 实例在将监听器添加到其内部监听器数组*之前*会触发自身的 `'newListener'` 事件。

注册了 `'newListener'` 事件的监听器将被传递事件名称并添加一个监听器的引用。

事实上，在事件触发前添加监听器会有一个微妙但重要的副作用：*任何额外*被注册成相同**名称**的监听器在被添加新的监听器前都会触发*内部*的 `'newListener'` 回调。

```javascript
const myEmitter = new MyEmitter();
// 只进行一次，所以不用担心无限循环
myEmitter.once('newListener', (event, listener) => {
    if (event === 'event') {
        // 在前面插入新的监听器
        myEmitter.on('event', () => {
            console.log('B');
        });
    }
});
myEmitter.on('event', () => {
    console.log('A');
});
myEmitter.emit('event');
// 打印：
//   B
//   A
```


## 'removeListener' 事件

添加：v0.9.3

- `eventName` {String} | {Symbol} 事件名

- `listener` {Function} 回调函数

`'removeListener'` 事件在监听器被移除*后*发出。


## EventEmitter.defaultMaxListeners

添加：v0.11.2

任何单一事件默认都可以注册最多 10 个监听器。可以使用 [emitter.setMaxListeners(n)](#emittersetmaxlistenersn) 方法来单个 `EventEmitter` 实例更改此限制。可以使用 `EventEmitter.defaultMaxListeners` 属性更改*所有* `EventEmitter` 实例的默认设置。

请谨慎设置 `EventEmitter.defaultMaxListeners` 属性，因为这个改变会影响到*所有*的 `EventEmitter` 实例，包括那些之前创建的实例。因而，调用 [emitter.setMaxListeners(n)](#emittersetmaxlistenersn) 仍然优于设置 `EventEmitter.defaultMaxListeners` 。

请注意，这不是一个硬性限制。`EventEmitter` 实例允许添加更多的监听器，但会向 `stderr` 输出跟踪警告：表明已经检测到一个 `possible EventEmitter memory leak` 错误。对于任何 `EventEmitter` 实例单独使用 `emitter.getMaxListeners()` 和 `emitter.setMaxListeners()` 方法可以暂时避免此警告：

``` javascript
emitter.setMaxListeners(emitter.getMaxListeners() + 1);
emitter.once('event', () => {
    // 处理一些事情
    emitter.setMaxListeners(Math.max(emitter.getMaxListeners() - 1, 0));
});
```

[--trace-warnings](../cli/options.md#tracewarnings) 命令行标志可用于显示此类警告的堆栈跟踪。

可以使用 [process.on('warning')](../process/process.md#warning-事件) 检查发出的警告，并具有额外的 `emitter`、`type` 和 `count` 属性，分别代表事件发生器的引用，事件的名称和附加的监听器的数量。它的 `name` 属
性设置为 `'MaxListenersExceededWarning'`。


## EventEmitter.listenerCount(emitter, eventName)

添加：v0.9.12  废弃：v4.0.0

> 稳定度：0 - 已废弃：请使用 [emitter.listenerCount()](#emitterlistenercounteventname) 替代。

返回给定 `emitter` 上注册的给定 `eventName` 的监听器数量的类方法。

``` javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {});
myEmitter.on('event', () => {});
console.log(EventEmitter.listenerCount(myEmitter, 'event'));
// 打印：2
```


## emitter.on(eventName, listener)

添加：v0.1.101

- `eventName` {String} | {Symbol} 事件名

- `listener` {Function} 回调函数

将侦听器函数添加到名为 `eventName` 的事件的 `listener` 数组的末尾。不会检测 `listener` 是否已被添加。多次调用传递了相同的 `eventName` 和 `listener` 的组合会导致 `listener` 被添加和调用多次。

``` javascript
server.on('connection', (stream) => {
    console.log('有人连接！');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。

默认情况下，事件监听器按照添加的顺序依次调用。`emitter.prependListener()` 方法可以用作将事件监听器添加到 `listeners` 数组开头的可选方法。

``` javascript
const myEE = new EventEmitter();
myEE.on('foo', () => console.log('a'));
myEE.prependListener('foo', () => console.log('b'));
myEE.emit('foo');
// 打印：
//   b
//   a
```


## emitter.once(eventName, listener)

添加：v0.3.0

- `eventName` {String} | {Symbol} 事件名

- `listener` {Function} 回调函数

给名为 `eventName` 的事件添加一个**一次性**的 `listener` 函数。下一次发出 `eventName` 事件时，此监听器将被移除，并在随后调用。

``` javascript
server.once('connection', (stream) => {
    console.log('哈，我们有第一个用户！');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。

默认情况下，事件监听器按照添加的顺序依次调用。`emitter.prependOnceListener()` 方法可以用作将事件监听器添加到 `listeners` 数组开头的可选方法。

``` javascript
const myEE = new EventEmitter();
myEE.on('foo', () => console.log('a'));
myEE.prependOnceListener('foo', () => console.log('b'));
myEE.emit('foo');
// 打印：
//   b
//   a
```


## emitter.addListener(eventName, listener)

添加：v0.1.26

[emitter.on(eventName, listener)](#emitteroneventname-listener) 的别名。


## emitter.prependListener(eventName, listener)

添加：v6.0.0

- `eventName` {String} | {Symbol} 事件名

- `listener` {Function} 回调函数

将监听器函数添加到名为 `eventName` 事件的 `listeners` 数组的*开头*。不会检测 `listener` 是否已被添加。多次调用传递了相同的 `eventName` 和 `listener` 的组合会导致 `listener` 被添加和调用多次。

``` javascript
server.prependListener('connection', (stream) => {
    console.log('有人连接！');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。


## emitter.prependOnceListener(eventName, listener)

添加：v6.0.0

- `eventName` {String} | {Symbol} 事件名

- `listener` {Function} 回调函数

将名为 `eventName` 事件的**一次性**的 `listener` 函数添加到 `listeners` 数组的*开头*。下一次发出 `eventName` 事件时，此监听器将被移除，并在随后调用。

``` javascript
server.prependOnceListener('connection', (stream) => {
    console.log('哈，我们有第一个用户！');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。


## emitter.removeListener(eventName, listener)

添加：v0.1.26

从名为 `eventName` 的事件的监听器数组中移除特定的 `listener`。

``` javascript
const callback = (stream) => {
    console.log('有人连接！');
};
server.on('connection', callback);
// ...
server.removeListener('connection', callback);
```

`removeListener` 最多只会从当前的监听器数组里移除一个监听器实例。如果任何单一的监听器多次添加特定的 `eventName` 到监听器数组中，必须多次调用 `removeListener` 才能移除每个实例。

请注意，一旦发出事件，所有关联到它的监听器将被按顺序依次触发。这也意味着，任何的 `removeListener()` 或 `removeAllListeners()` 在调用*触发后*和最后一个监听器*执行完毕前*不会从 `emit()` 过程中移除它们。随后的事件会像预期的那样发生。

``` javascript
const myEmitter = new MyEmitter();

const callbackA = () => {
	console.log('A');
	myEmitter.removeListener('event', callbackB);
};

const callbackB = () => {
	console.log('B');
};

myEmitter.on('event', callbackA);

myEmitter.on('event', callbackB);

// 在 callbackA 中移除监听器 callbackB，但它仍然会被调用
// 内部监听器数组此时触发 [callbackA, callbackB]
myEmitter.emit('event');
// 打印：
//   A
//   B

// callbackB 现在被移除了
// 内部监听器数组为 [callbackA]
myEmitter.emit('event');
// 打印：
//   A
```

因为监听器是使用内部数组进行管理的，所以调用它将改变在监听器被移除*后*注册的任何监听器的位置索引。虽然这不会影响监听器的调用顺序，但也意味着由 [emitter.listeners()](#emitterlistenerseventname) 方法返回的任何监听器副本都将需要被重新创建。

返回一个当前 `EventEmitter` 的引用以便链式调用。


## emitter.removeAllListeners([eventName])

添加：v0.1.26

移除全部或某些特定 `eventName` 的监听器。

请注意，在代码中移除其他地方添加的监听器是一个不好的做法，尤其是由其他组件或模块（如，套接字或文件流）创建的 `EventEmitter` 实例。

返回一个当前 `EventEmitter` 的引用以便链式调用。


## emitter.emit(eventName[, ...args])

添加：v0.1.26

按照监听器的注册顺序同步调用每个以 `eventName` 注册的监听器，并将额外的参数传递给它们。

如果事件有监听器存在就返回 `true` ，否则返回 `false` 。


## emitter.listeners(eventName)

添加：v0.1.26

返回名为 `eventName` 的事件的监听器数组的副本。

``` javascript
server.on('connection', (stream) => {
	console.log('有人连接！');
});
console.log(util.inspect(server.listeners('connection')));
// 打印：[ [Function] ]
```


## emitter.listenerCount(eventName)

添加：v3.2.0

- `eventName` {String} | {Symbol} 事件名

返回正在监听名为 `eventName` 的事件的监听器数量。


## emitter.setMaxListeners(n)

添加：v0.3.5

默认情况下，如果为特定事件添加了超过 `10` 个监听器，`EventEmitter` 将打印警告。此限制在寻找内存泄露时非常有用。显然，并不是所有的事件都要被仅限为 `10` 个。`emitter.setMaxListeners()` 方法允许修改特定的 `EventEmitter` 实例的限制数量。如果不想限制监听器的数量，可以将该值设置为 `Infinity` （或 `0`）。

返回一个当前 `EventEmitter` 的引用以便链式调用。


## emitter.getMaxListeners()

添加：v1.0.0

返回当前 `EventEmitter` 实例的最大监听器数量，该值可以通过 [emitter.setMaxListeners(n)](#emittersetmaxlistenersn) 或 [EventEmitter.defaultMaxListeners](#eventemitterdefaultmaxlisteners) 的默认值设置。