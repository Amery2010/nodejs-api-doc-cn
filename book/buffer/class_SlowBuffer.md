# SlowBuffer类

* [new SlowBuffer(size)](#new_SlowBuffer)

--------------------------------------------------


返回一个不被池管理的 Buffer 。

为了避免创建大量独立分配的 Buffer 带来的垃圾回收开销，默认情况下小于 4KB 的空间都是切割自一个较大的独立对象。这种策略既提高了性能也改善了内存使用，因为 V8 不需要跟踪和清理很多 `Persistent` 对象。

当开发者需要将池中一小块数据保留不确定的一段时间，较为妥当的办法是用 `SlowBuffer` 创建一个不被池管理的 Buffer 实例并将相应数据拷贝出来。

```javascript
// need to keep around a few small chunks of memory
const store = [];

socket.on('readable', () => {
    var data = socket.read();
    // allocate for retained data
    var sb = SlowBuffer(10);
    // copy the data into the new allocation
    data.copy(sb, 0, 0, 10);
    store.push(sb);
});
```

`SlowBuffer` 应该仅作为开发者已经观察到他们的应用保留了过度的内存时的最后手段。


<div id="new_buffer_size" class="anchor"></div>
## new SlowBuffer(size)

- `size` {Number}

分配一个 `size` 字节大小的新 `SlowBuffer`。`size` 必须小于等于 `require('buffer').kMaxLength`（在64位架构上 `kMaxLength` 的大小是 `(2^31)-1`）的值，否则将抛出一个 [RangeError](../errors/class_RangeError.md#) 的错误。如果 `size` 小于 0 将创建一个特定的 0 长度（zero-length ）的 SlowBuffer。

`SlowBuffer` 实例的底层内存是*没被初始化过*的。新创建的 `SlowBuffer` 的内容是*未知*的，并*可能包含敏感数据*。通过使用 `buf.fill(0)` 将一个 `SlowBuffer` 初始化为零。

```javascript
const SlowBuffer = require('buffer').SlowBuffer;
const buf = new SlowBuffer(5);
console.log(buf);
// <Buffer 78 e0 82 02 01>
// (octets will be different, every time)
buf.fill(0);
console.log(buf);
// <Buffer 00 00 00 00 00>
```