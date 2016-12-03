# Timeout 类

* [timeout.unref()](#timeoutunref)
* [timeout.ref()](#timeoutref)

--------------------------------------------------

此对象在内部创建，并从 [setTimeout()](./scheduling_timers.md#settimeoutcallback-delay-args) 和 [setInterval()](./scheduling_timers.md#setintervalcallback-delay-args) 返回。它可以分别传递给 [clearTimeout()](./cancelling_timers.md#cleartimeouttimeout) 或 [clearInterval()](./cancelling_timers.md#clearintervaltimeout) 以便取消计划的动作。

默认情况下，当使用 [setTimeout()](./scheduling_timers.md#settimeoutcallback-delay-args) 或 [setInterval()](./scheduling_timers.md#setintervalcallback-delay-args) 调度定时器时，只要定时器仍处于活动状态，Node.js 事件循环将继续运行。每个由这些函数返回的 `Timeout` 对象都可以导出可用于控制此默认行为的 `timeout.ref()` 和 `timeout.unref()` 函数。


## timeout.unref()

添加：v0.9.1

当被调用时，活动的 `Timeout` 对象将不需要 Node.js 事件循环保持活动。如果没有其他活动保持事件循环运行，那么该进程可以在 `Timeout` 对象的回调被调用之前退出。调用 `timeout.unref()` 多次将没有效果。

*注意*：调用 `timeout.unref()` 创建一个内部定时器，将唤醒 Node.js 的事件循环。创建太多这类定时器可能会对 Node.js 应用程序的性能产生负面影响。

返回对 `Timeout` 的引用。


## timeout.ref()

添加：v0.9.1

调用时，只要 `Timeout` 处于活动状态就请求 Node.js *不要*退出事件循环。调用 `timeout.ref()` 多次将没有效果。

*注意*：默认情况下，所有 `Timeout` 对象都处于 `'ref'd'` 状态，通常不需要调用 `timeout.ref()`，除非之前调用了 `timeout.unref()`。

返回对 `Timeout` 的引用。