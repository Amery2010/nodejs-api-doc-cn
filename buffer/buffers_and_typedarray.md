# Buffers和类型数组

Buffers 同样是 `Uint8Array` 类型数组（TypedArray）的实例。但是，和 ECMAScript 2015 中的 `TypedArray` 规范还是有些微妙的不同之处。例如，当 `ArrayBuffer#slice()` 创建了一个切片的副本时，[Buffer#slice()](./class_Buffer.md#slice) 的实现是在现有的 Buffer 上不经过拷贝直接进行创建，这也使得 `Buffer#slice()` 更为有效。

在以下的注意事项下，从一个 Buffer 里创建一个新的类型数组（TypedArray）也是有可能的：

1. 将 Buffer 对象的内存以不共享的方式拷贝到这个类型数组（TypedArray）中。

2. 将 Buffer 对象的内存解释执行为一个不同元素的数组，并且不是目标类型的字节数组。这就像，`new Uint32Array(Buffer.from([1,2,3,4]))` 创建了一个4元素的 `Uint32Array` 的包含元素 `[1,2,3,4]` ，而不是 `Uint32Array` 包含一个单一的元素 `[0x1020304]` 或 `[0x4030201]` 。

也可以通过类型数组对象的 `.buffer` 属性创建一个新的 Buffer ，作为共享同一分配的内存的类型数组实例：

```javascript
const arr = new Uint16Array(2);
arr[0] = 5000;
arr[1] = 4000;

const buf1 = Buffer.from(arr); // copies the buffer
const buf2 = Buffer.from(arr.buffer); // shares the memory with arr;

console.log(buf1);
// Prints: <Buffer 88 a0>, copied buffer has only two elements
console.log(buf2);
// Prints: <Buffer 88 13 a0 0f>

arr[1] = 6000;
console.log(buf1);
// Prints: <Buffer 88 a0>
console.log(buf2);
// Prints: <Buffer 88 13 70 17>
```

请注意，当通过类型数组的 `.buffer` 属性创建一个 Buffer 时，有可能只能通过传入 `byteOffset` 和 `length` 参数使用底层类型数组的一部分。

```javascript
const arr = new Uint16Array(20);
const buf = Buffer.from(arr.buffer, 0, 16);
console.log(buf.length);
// Prints: 16
```

`Buffer.from()` 和 [TypedArray.from()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/from) （如，`Uint8Array.from()`）有着不同的签名和实现。具体而言，类型数组的变种接受第二参数，在类型数组的每个元素上调用一个映射函数。

但 `Buffer.from()` 不支持使用一个映射函数：

* [Buffer.from(array)](./class_Buffer.md#Buffer_from_array)
 
* [Buffer.from(buffer)](./class_Buffer.md#Buffer_from_buffer)
 
* [Buffer.from(arrayBuffer[, byteOffset[, length]])](./class_Buffer.md#Buffer_from_arrayBuffer)
 
* [Buffer.from(str[, encoding])](./class_Buffer.md#Buffer_from_str)