# 计划定时器

* [setTimeout(callback, delay[, ...args])](#settimeoutcallback-delay-args)
* [setInterval(callback, delay[, ...args])](#setintervalcallback-delay-args)
* [setImmediate(callback[, ...args])](#setimmediatecallback-args)

--------------------------------------------------

Node.js 中的计时器是一种会在一段时间后调用给定的函数的内部构造。定时器函数会在何时被调用，取决于用来创建定时器的方法以及 Node.js 事件循环是否正在做其他工作。


## setTimeout(callback, delay[, ...args])

添加：v0.0.1

