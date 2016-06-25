# 特殊错误属性

任何时候一个 `Error` 对象通过了域（domain）路由，一些额外的字段会被添加到它上面。

* `error.domain` 首先处理错误的域（domain）。

* `error.domainEmitter` 触发了错误对象的 `'error'` 事件的事件触发器。

* `error.domainBound` 绑定在域（domain）上的回调函数，并把错误作为第一个参数传过去。

* `error.domainThrown` 一个布尔值，指示错误是否被抛出、触发或传递给绑定的回调函数。