# 事件(Events)

* [EventEmitter 类](./class_EventEmitter.md)
* [错误事件](./error_events.md)
* [绑定一次性事件](./handling_events_only_once.md)
* [将参数和 this 传递给监听器](./passing_arguments_and_this_to_listeners.md)
* [异步和同步](./asynchronous_vs_synchronous.md)

------------------------------------------------------------

> 稳定度：2 - 稳定

大多数 Node.js 核心 API 都是采用惯用的异步事件驱动架构，通过某种对象（称为“触发器”）定期触发命名事件导致函数对象（“监听器”）被调用。

例如：一个 [net.Server](../net/class_net_Server.md) 对象会在每次有新连接时触发一个事件；一个 [fs.readStream](../stream/api_for_stream_consumers.md#readStream) 对象会在文件被打开时触发一个事件；一个 [stream](../stream/api_for_stream_consumers.md#stream) 对象在每次数据可读时触发一个事件。

所有能触发事件的对象都是 `events.EventEmitter` 类的实例。这些对象拥有一个 `eventEmitter.on()` 函数允许一个或多个函数附加到由该对象触发的命名事件上。通常情况下，事件名称是驼峰命名 (camel-cased) 的字符串，但也可以使用任何有效的 JavaScript 属性键值。

每当 `EventEmitter` 触发一个事件，所有附加到特定事件上的函数都是*同步*调用的。所有由监听器回调返回的值都会被*忽略*并会被丢弃。

以下例子展示了一个只有单个监听器的简单的 `EventEmitter` 实例。`eventEmitter.on()` 用于注册监听器，`eventEmitter.emit()` 用于触发事件。

```javascript
const EventEmitter = require('events');
const util = require('util');

function MyEmitter() {
    EventEmitter.call(this);
}
util.inherits(MyEmitter, EventEmitter);

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
    console.log('an event occurred!');
});
myEmitter.emit('event');
```

任何对象通过继承都能变成一个 `EventEmitter` 对象。上面的例子通过使用 `util.inherits()` 方法实现了传统的 Node.js 风格的原型继承。然而，最好使用 ES6 的类方法。

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
    console.log('an event occurred!');
});
myEmitter.emit('event');
```