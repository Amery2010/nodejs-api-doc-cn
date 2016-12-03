# 定时器(Timers)

* [预定定时器](./timers/scheduling_timers.md)
* [取消定时器](./timers/cancelling_timers.md)
* [Timeout 类](./timers/class_Timeout.md)
* [Immediate 类](./timers/class_Immediate.md)

--------------------------------------------------

> 稳定度：3 - 已锁定

`timer` 模块暴露了一个全局的 API 用于调度在某个未来时间段内调用的函数。因为定时器函数是全局的，所以你没有必要调用 `require('timers')` 来使用该 API。

Node.js 中的计时器函数实现了与 Web 浏览器提供的定时器类似的 API，但它使用了基于 [Node.js 事件循环](https://github.com/nodejs/node/blob/master/doc/topics/event-loop-timers-and-nexttick.md)构建的不同内部实现。