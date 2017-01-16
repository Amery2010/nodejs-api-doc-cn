# 事件(Events)

* [EventEmitter 类](./class_EventEmitter.md)
* [错误事件](./error_events.md)
* [绑定一次性事件](./handling_events_only_once.md)
* [将参数和 `this` 传递给监听器](./passing_arguments_and_this_to_listeners.md)
* [异步和同步](./asynchronous_vs_synchronous.md)

------------------------------------------------------------

> 稳定度：2 - 稳定

大多数 Node.js 核心 API 都是采用惯用的异步事件驱动架构，其中某些类型的对象（称为“触发器”）周期性地发出命名事件来调用函数对象（“监听器”）。

例如：[net.Server](../net/class_net_Server.md) 对象会在每次有新连接时发出事件；[fs.readStream](../stream/api_for_stream_consumers.md#readStream) 对象会在文件被打开时发出事件；[stream](../stream/api_for_stream_consumers.md#stream) 对象在每当在数据可读时发出事件。

所有能发出事件的对象都是 `EventEmitter` 类的实例。这些对象公开了一个 `eventEmitter.on()` 函数，它允许将一个或多个函数附加到由该对象发出的命名事件上。通常情况下，事件名称是小写驼峰式 (camel-cased) 字符串，但也可以使用任何有效的 JavaScript 属性名。

每当 `EventEmitter` 发出事件，所有附加到特定事件上的函数都被*同步*调用。所有由监听器回调返回的值都会被*忽略*并被丢弃。

以下例子展示了一个只有单个监听器的简单的 `EventEmitter` 实例。`eventEmitter.on()` 用于注册监听器，`eventEmitter.emit()` 用于触发事件。

``` javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
	console.log('发生了一个事件!');
});
myEmitter.emit('event');
```