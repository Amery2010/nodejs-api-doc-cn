# 方法和属性

* [buffer.INSPECT_MAX_BYTES](#INSPECT_MAX_BYTES)

--------------------------------------------------


<div id="INSPECT_MAX_BYTES" class="anchor"></div>
## buffer.INSPECT_MAX_BYTES

- {Number} 默认：50

在调用 `buffer.inspect()` 时返回最大字节数。这个数值可以被用户模块重写。查阅 [util.inspect()](../util/util.md#inspect)可以获得更多关于 `buffer.inspect()` 的行为细节。

注意，这个属性只存在于使用 `require('buffer')` 返回的 `buffer` 模块中，在全局的 Buffer 和 Buffer 实例中是不存在的。