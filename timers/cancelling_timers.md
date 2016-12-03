# 取消定时器

* [clearTimeout(timeout)](#cleartimeouttimeout)
* [clearInterval(timeout)](#clearintervaltimeout)
* [clearImmediate(immediate)](#clearimmediateimmediate)

--------------------------------------------------

每个 [setTimeout()](./scheduling_timers.md#settimeoutcallback-delay-args)、[setInterval()](./scheduling_timers.md#setintervalcallback-delay-args) 和 [setImmediate()](./scheduling_timers.md#setimmediatecallback-args) 都会返回表示预定计时器的对象。这些可用于取消定时器并防止其触发。


## clearTimeout(timeout)

添加：v0.0.1

* `timeout` {Timeout} 由 [setTimeout()](./scheduling_timers.md#settimeoutcallback-delay-args) 返回的 `Timeout` 对象。

取消由 [setTimeout()](./scheduling_timers.md#settimeoutcallback-delay-args) 创建的 `Timeout` 对象。


## clearInterval(timeout)

添加：v0.0.1

* `timeout` {Timeout} 由 [setInterval()](./scheduling_timers.md#setintervalcallback-delay-args) 返回的 `Timeout` 对象。

取消由 [setInterval()](./scheduling_timers.md#setintervalcallback-delay-args) 创建的 `Timeout` 对象。


## clearImmediate(immediate)

添加：v0.9.1

* `immediate` {Immediate} 由 [setImmediate()](./scheduling_timers.md#setimmediatecallback-args) 返回的 `Immediate` 对象。

取消由 [setImmediate()](./scheduling_timers.md#setimmediatecallback-args) 创建的 `Immediate` 对象。