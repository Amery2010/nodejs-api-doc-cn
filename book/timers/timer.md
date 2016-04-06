# 方法和属性

* [setTimeout(callback, delay[, arg][, ...])](#setTimeout)
* [clearTimeout(timeoutObject)](#clearTimeout)
* [setInterval(callback, delay[, arg][, ...])](#setInterval)
* [clearInterval(intervalObject)](#clearInterval)
* [setImmediate(callback[, arg][, ...])](#setImmediate)
* [clearImmediate(immediateObject)](#clearImmediate)
* [ref()](#ref)
* [unref()](#unref)

--------------------------------------------------


<div id="setTimeout" class="anchor"></div>
## setTimeout(callback, delay[, arg][, ...])

在 `delay` 毫秒后执行一次回调函数 `callback` 。返回一个可能被 [clearTimeout](#clearTimeout) 用到的 `timeoutObject` 。其他可选参数可以传递给回调函数。

请务必注意，您的回调有可能不会在准确的 `delay` 毫秒后被调用。Node.js 不保证回调被触发的精确时间和顺序。回调会在尽可能接近所指定的时间上被调用。

根据浏览器默认行为，当你使用大于 2147483647 毫秒（约25天）或小于 1 毫秒的延迟时，`delay` 会被设置为 1 ，超时将立即执行。


<div id="clearTimeout" class="anchor"></div>
## clearTimeout(timeoutObject)

阻止一个由 [setTimeout](#setTimeout) 创建的 `timeoutObject` 触发。


<div id="setInterval" class="anchor"></div>
## setInterval(callback, delay[, arg][, ...])

每隔 `delay` 毫秒执行一次回调函数 `callback` 。返回一个可能被 [clearInterval](#clearInterval) 用到的 `intervalObject` 。其他可选参数可以传递给回调函数。

根据浏览器默认行为，当你使用大于 2147483647 毫秒（约25天）或小于 1 毫秒的延迟时，Node.js将使用 1 作为延迟。


<div id="clearInterval" class="anchor"></div>
## clearInterval(intervalObject)

阻止一个由 [setInterval](#setInterval) 创建的 `intervalObject` 触发。


<div id="setImmediate" class="anchor"></div>
## setImmediate(callback[, arg][, ...])

在 I/O 事件回调之后以及 [setTimeout](#setTimeout) 和 [setInterval](#setInterval) 之前“立即”执行  `callback` 。返回一个可能被 [clearImmediate](#clearImmediate) 用到的 `immediateObject` 。其他可选参数可以传递给回调函数。


<div id="clearImmediate" class="anchor"></div>
## clearImmediate(immediateObject)

阻止一个由 [setImmediate](#setImmediate) 创建的 `immediateObject` 触发。


<div id="ref" class="anchor"></div>
## ref()

如果您之前 `unref()` 了一个定时器，您可以调用 `ref()` 来明确要求定时器让程序保持运行。如果定时器已被 `ref` 那么再次调用 `ref` 不会产生其它影响。

返回定时器对象。


<div id="unref" class="anchor"></div>
## unref()

[setTimeout](#setTimeout) 和 [setInterval](#setInterval) 所返回的值都包含 `timer.unref()` 方法，该方法允许您创建一个活动的但当它是事件循环中仅剩的项目时不会保持程序运行的定时器。如果定时器已被 `unref` ，再次调用 `unref` 不会产生其它影响。

在使用 [setTimeout](#setTimeout) 的情况下，`unref` 会创建另一个独立的定时器，并唤醒事件循环。创建太多这种定时器可能会影响事件循环的性能，**慎用**。

返回定时器对象。