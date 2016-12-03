# 预定定时器

* [setTimeout(callback, delay[, ...args])](#settimeoutcallback-delay-args)
* [setInterval(callback, delay[, ...args])](#setintervalcallback-delay-args)
* [setImmediate(callback[, ...args])](#setimmediatecallback-args)

--------------------------------------------------

Node.js 中的计时器是一种会在一段时间后调用给定的函数的内部构造。定时器函数会在何时被调用，取决于用来创建定时器的方法以及 Node.js 事件循环是否正在做其他工作。


## setTimeout(callback, delay[, ...args])

添加：v0.0.1

* `callback` {Function} 当定时器到点时回调的函数。

* `delay` {Number} 在调用 `callback` 之前等待的毫秒数。

* `...args` {Any} 在调用 `callback` 时要传递的可选参数。

在 `delay` 毫秒之后预定执行一次性的 `callback`。返回一个用于 [clearTimeout()](./cancelling_timers.md#cleartimeouttimeout) 的 `Timeout`。

`callback` 可能不会精确地在 `delay` 毫秒被调用。Node.js 不能保证回调被触发的确切时间，也不能保证它们的顺序。回调会在尽可能接近所指定的时间上调用。

*注意*：当 `delay` 大于 `2147483647` 或小于 `1` 时，`delay` 会被设置为 `1`。

如果 `callback` 不是一个函数，将会抛出一个 [TypeError](../errors/class_TypeError.md)。


## setInterval(callback, delay[, ...args])

添加：v0.0.1

* `callback` {Function} 当定时器到点时回调的函数。

* `delay` {Number} 在调用 `callback` 之前等待的毫秒数。

* `...args` {Any} 在调用 `callback` 时要传递的可选参数。

预定每隔 `delay` 毫秒重复执行 `callback`。返回一个用于 [clearInterval()](./cancelling_timers.md#clearintervaltimeout) 的 `Timeout`。

*注意*：当 `delay` 大于 `2147483647` 或小于 `1` 时，`delay` 会被设置为 `1`。

如果 `callback` 不是一个函数，将会抛出一个 [TypeError](../errors/class_TypeError.md)。


## setImmediate(callback[, ...args])

添加：v0.9.1

* `callback` {Function} 在当前的 Node.js 事件循环回合结束时调用该函数。

* `...args` {Any} 在调用 `callback` 时要传递的可选参数。

预定“立即”执行 `callback`，它是在 I/O 事件的回调之后并在使用 [setTimeout()](settimeoutcallback-delay-args) 和 [setInterval()](setintervalcallback-delay-args) 创建的计时器之前被触发。返回一个用于 [clearImmediate()](./cancelling_timers.md#clearimmediateimmediate) 的 `Immediate`。

当多次调用 `setImmediate()` 时，回调函数会按照它们的创建顺序依次执行。每个事件循环迭代都会处理整个回调队列。如果立即定时器正在执行回调中排队，那么该定时器直到下一个事件循环迭代之前将不会被触发。

如果 `callback` 不是一个函数，将会抛出一个 [TypeError](../errors/class_TypeError.md)。