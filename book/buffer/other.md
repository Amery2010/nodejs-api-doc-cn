# Buffer.from(), Buffer.alloc(), and Buffer.allocUnsafe()

* [是什么使得 Buffer.allocUnsafe(size) “不安全”？](#why)

--------------------------------------------------


由于历史原因，`Buffer` 实例通过 `Buffer` 构造函数创建，它基于提供的不同参数分配返回不同的 `Buffer` ：

* 给 `Buffer()` 的第一个参数传参（如，`new Buffer(10)`），通过指定的大小分配一个新的 Buffer 。给这样的 Buffer 分配的内存是*没有初始化*过的，并*会包含敏感数据*。这样的 `Buffer` 对象*必须手动*通过 [buf.fill(0)](./class_Buffer.md#fill) 初始化或写满这个 `Buffer` 。虽然这种行为是为了提高性能而*故意为之*的，开发经验已经证明对于是创造一个更慢但很安全的 `Buffer` 还是创建一个快速但未初始化的 `Buffer` 之间需要更加明确的区分。

* 通过给第一个参数传字符串、数组或 Buffer ，可以将所传对象的数据拷贝当前 Buffer 中。

* 传一个 `ArrayBuffer` 返回一个与给定的 `ArrayBuffer` 共享分配的内存的 `Buffer` 。

因为 `new Buffer()` 行为会根据第一个参数所传值的类型不同而显著改变，所以应用程序如果没有适当地验证给 `new Buffer()` 传的输入参数，或未能适当地初始化新分配的 `Buffer` 的内容，会给他们的代码带来安全性和可靠性方面的问题。

为了使创建的 `Buffer` 对象更可靠，更不容易出错，新的 `Buffer.from()`、 `Buffer.alloc()` 和  `Buffer.allocUnsafe()` 方法作为创建 `Buffer` 实例的替代手段而相继出台。

开发者*应当把所有正在使用的 `new Buffer()` 构造函数迁移到这些新的API之一*：

* [Buffer.from(array)](./class_Buffer.md#Buffer_from_array) 返回一个包含所提供的 8位字节*的副本*的新 `Buffer` 。

* [Buffer.from(arrayBuffer[, byteOffset[, length]])](./class_Buffer.md#Buffer_from_arrayBuffer) 返回一个与给定的 `ArrayBuffer` *共享*分配的内存的 `Buffer` 。

* [Buffer.from(buffer)](./class_Buffer.md#Buffer_from_buffer) 返回一个包含所提供的 `Buffer` *的副本*的新 `Buffer` 。

* [Buffer.from(str[, encoding])](./class_Buffer.md#Buffer_from_str)  返回一个包含所提供的字符串的*副本*的新 `Buffer` 。

* [Buffer.alloc(size[, fill[, encoding]])](./class_Buffer.md#Buffer_alloc) 返回一个指定大小的被填满的 `Buffer` 实例。这种方法会比 [Buffer.allocUnsafe(size)](./class_Buffer.md#Buffer_allocUnsafe) 显著地慢，但可确保新创建的 `Buffer` 绝不会包含旧的和潜在的敏感数据。

* [Buffer.allocUnsafe(size)](./class_Buffer.md#Buffer_allocUnsafe) 返回一个指定 `size` 的 `Buffer` ，但它的内容*必须*被 [buf.fill(0)](./class_Buffer.md#fill) 初始化或完全写满。

被 `Buffer.allocUnsafe(size)` 返回的 `Buffer` 实例，如果它的 `size` 小于或等于 `Buffer.poolSize` 的一半，可能被分配进一个共享的内部内存池。


<div id="why" class="anchor"></div>
### 是什么使得 Buffer.allocUnsafe(size) “不安全”？

当调用 `Buffer.allocUnsafe()` 时，被分配的内存段是*没被初始化*（它不是被零填充的）过的。虽然这样的设计使得内存的分配相当快，但已分配的存储段可能包含潜在的敏感的旧数据。使用通过 `Buffer.allocUnsafe(size)` 创建没有被*完全*覆写内存的 `Buffer` ，在 `Buffer` 内存是可读的情况下，可能泄露它的旧数据。

虽然在使用 `Buffer.allocUnsafe()` 时有明显的性能优势，但必须额外小心，以避免给应用程序引入安全漏洞。