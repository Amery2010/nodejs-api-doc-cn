# EventEmitter类

* ['newListener'事件](#newListener)
* ['removeListener'事件](#removeListener)
* [EventEmitter.defaultMaxListeners](#EventEmitter_defaultMaxListeners)
* [EventEmitter.listenerCount(emitter, eventName)](#EventEmitter_listenerCount)
* [emitter.on(eventName, listener)](#on)
* [emitter.once(eventName, listener)](#once)
* [emitter.addListener(eventName, listener)](#addListener)
* [emitter.removeListener(eventName, listener)](#removeListener)
* [emitter.removeAllListeners([eventName])](#removeAllListeners)
* [emitter.emit(eventName[, arg1][, arg2][, ...])](#emit)
* [emitter.listeners(eventName)](#listeners)
* [emitter.listenerCount(eventName)](#listenerCount)
* [emitter.setMaxListeners(n)](#setMaxListeners)
* [emitter.getMaxListeners()](#getMaxListeners)

--------------------------------------------------


`EventEmitter` 类由 `events` 模块定义和暴露：

```javascript
const EventEmitter = require('events');
```

所有的事件触发器都会在新的监听器被添加时触发 `'newListener'` 事件；在一个监听器被移除时触发 `'removeListener'` 事件。


<div id="newListener" class="anchor"></div>
## 'newListener' 事件

- `eventName` {String} | {Symbol} 被监听的事件的名称

- `listener` {Function} 事件处理函数

`EventEmitter` 实例在一个监听器添加到它内部的监听器数组*前*会触发自身的 `'newListener'` 事件。

注册了 `'newListener'` 事件的监听器将被传递事件名称并添加一个监听器的引用。

事实上，在事件触发前添加监听器会有一个微妙但重要的副作用：*任何额外*被注册成相同**名称**的监听器在被添加新的监听器前都会触发*内部*的 `'newListener'` 回调。

```javascript
const myEmitter = new MyEmitter();
// Only do this once so we don't loop forever
myEmitter.once('newListener', (event, listener) => {
    if (event === 'event') {
        // Insert a new listener in front
        myEmitter.on('event', () => {
            console.log('B');
        });
    }
});
myEmitter.on('event', () => {
    console.log('A');
});
myEmitter.emit('event');
// Prints:
//   B
//   A
```


<div id="removeListener" class="anchor"></div>
## 'removeListener' 事件

- `eventName` {String} | {Symbol} 被监听的事件的名称

- `listener` {Function} 事件处理函数

`'removeListener'` 事件在一个监听器被移除*后*触发。


<div id="EventEmitter_defaultMaxListeners" class="anchor"></div>
## EventEmitter.defaultMaxListeners

任何单一事件默认都可以注册最多 10 个监听器。每个 `EventEmitter` 实例都可以使用 [emitter.setMaxListeners(n)](#setMaxListeners) 方法来破除这个限制。可以使用 `EventEmitter.defaultMaxListeners` 属性改变*所有* `EventEmitter` 实例的默认设置。

请谨慎设置 `EventEmitter.defaultMaxListeners` 属性，因为这个改变会影响到*所有*的 `EventEmitter` 实例，包括那些之前创建的实例。因而，调用 [emitter.setMaxListeners(n)](#setMaxListeners) 仍然优于设置 `EventEmitter.defaultMaxListeners` 。

请注意，这不是一个硬性限制。`EventEmitter` 实例允许添加更多的监听器但会输出一个 `stderr` 的跟踪警告：检测到一个 `possible EventEmitter memory leak` 错误。对于任何 `EventEmitter` 实例单独使用 `emitter.getMaxListeners()` 和 `emitter.setMaxListeners()` 方法可以暂时避免此警告：

```javascript
emitter.setMaxListeners(emitter.getMaxListeners() + 1);
emitter.once('event', () => {
    // do stuff
    emitter.setMaxListeners(Math.max(emitter.getMaxListeners() - 1, 0));
});
```


<div id="EventEmitter_listenerCount" class="anchor"></div>
## EventEmitter.listenerCount(emitter, eventName)

> 稳定度：0 - 已废弃：请使用 [emitter.listenerCount()](#listenerCount) 替代。

一个返回给定 `emitter` 上注册的给定 `eventName` 的监听器数量的类方法。

```javascript
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {});
myEmitter.on('event', () => {});
console.log(EventEmitter.listenerCount(myEmitter, 'event'));
// Prints: 2
```


<div id="on" class="anchor"></div>
## emitter.on(eventName, listener)

在监听器数组末尾添加名为 `eventName` 的 `listener` 函数，不会检测 `listener` 是否已经被添加。通过重复传递相同的 `eventName` 和 `listener` 组合会导致 `listener` 被添加和调用多次。

```javascript
server.on('connection', (stream) => {
    console.log('someone connected!');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。


<div id="once" class="anchor"></div>
## emitter.once(eventName, listener)

给名为 `eventName` 的事件添加一个**一次性**的 `listener` 函数。这个监听器仅在下次 `eventName` 触发时被激活，随后又被删除。

```javascript
server.once('connection', (stream) => {
    console.log('Ah, we have our first user!');
});
```

返回一个当前 `EventEmitter` 的引用以便链式调用。


<div id="addListener" class="anchor"></div>
## emitter.addListener(eventName, listener)

[emitter.on(eventName, listener)](#on) 的别名。


<div id="removeListener" class="anchor"></div>
## emitter.removeListener(eventName, listener)

从监听器数组中移除名为 `eventName` 的特定的 `listener` 。

```javascript
var callback = (stream) => {
    console.log('someone connected!');
};
server.on('connection', callback);
// ...
server.removeListener('connection', callback);
```

`removeListener` 最多只会从当前的监听器数组里移除一个监听器实例。如果任何单一的监听器多次添加特定的 `eventName` 到监听器数组中，必须多次调用 `removeListener` 才能移除每个实例。

请注意，一旦一个事件被触发，所有关联到它的监听器都能在那时依次触发。这也意味着，任何的 `removeListener()` 或 `removeAllListeners()` 在调用*触发后*和最后一个监听器*执行完毕前*不会在 `emit()` 过程中移除它们。随后的事件会像预期的那样发生。

```javascript
const myEmitter = new MyEmitter();

var callbackA = () => {
    console.log('A');
    myEmitter.removeListener('event', callbackB);
};

var callbackB = () => {
    console.log('B');
};

myEmitter.on('event', callbackA);

myEmitter.on('event', callbackB);

// callbackA removes listener callbackB but it will still be called.
// Internal listener array at time of emit [callbackA, callbackB]
myEmitter.emit('event');
// Prints:
//   A
//   B

// callbackB is now removed.
// Internal listener array [callbackA]
myEmitter.emit('event');
// Prints:
//   A
```

因为监听器通过一个内部数组进行管理，调用该方法会在监听器被*移除后*改变其中任何已注册的监听器的索引值。虽然这不会影响监听器的调用顺序，但也意味着由 [emitter.listeners()](#listeners) 方法返回的任何监听器副本都将需要被重新创建。

返回一个当前 `EventEmitter` 的引用以便链式调用。

<div id="removeAllListeners" class="anchor"></div>
## emitter.removeAllListeners([eventName])

移除全部或某些特定 `eventName` 的监听器。

请注意，在代码中移除其他地方添加的监听器是一个不好的做法，尤其是由其他组件或模块（如，套接字或文件流）创建的 `EventEmitter` 实例。

返回一个当前 `EventEmitter` 的引用以便链式调用。


<div id="emit" class="anchor"></div>
## emitter.emit(eventName[, arg1][, arg2][, ...])

按照监听器的注册顺序同步调用每个以 `eventName` 注册的监听器，并将额外的参数传递给它们。

如果事件有监听器存在就返回 `true` ，否则返回 `false` 。


<div id="listeners" class="anchor"></div>
## emitter.listeners(eventName)

返回名为 `eventName` 的事件的监听器数组的副本。


<div id="listenerCount" class="anchor"></div>
## emitter.listenerCount(eventName)

- `eventName` {Value} 被监听的事件名

返回正在监听名为 `eventName` 的事件的监听器数量。


<div id="setMaxListeners" class="anchor"></div>
## emitter.setMaxListeners(n)

在默认情况下，`EventEmitter` 会在多于 `10` 个监听器监听某个事件的时候出现警告，此限制在寻找内存泄露时非常有用。很显然，并不是所有的事件都要被仅限为 `10` 个。`emitter.setMaxListeners()` 方法允许修改特定的 `EventEmitter` 实例的限制数量。如果想要不限制监听器的数量，可以将这个值设置为 `Infinity` （或 `0`）。

返回一个当前 `EventEmitter` 的引用以便链式调用。


<div id="getMaxListeners" class="anchor"></div>
## emitter.getMaxListeners()

返回当前 `EventEmitter` 实例的最大监听器数量，该值可能是通过 [emitter.setMaxListeners(n)](#setMaxListeners) 设置的值或 [EventEmitter.defaultMaxListeners](#EventEmitter_defaultMaxListeners) 默认值。