# Buffer(Buffer)

> 稳定度：2 - 稳定

随着 ECMAScript 2015（ES6）推出了 `TypedArray` ，JavaScript 语言已经没有机制用于读取或操纵二进制数据流了。`Buffer` 类被采纳为 Node.js API 的一部分使得在 TCP 流和文件系统操作等的上下文中与八位字节流进行交互成为可能。

目前 `TypedArray` 已经被添加进 ES6 中，`Buffer` 类以一种更加优化和适用于 Node.js 用例的方式实现了 `Uint8Array` API。

`Buffer` 类的实例类似于整数数组，但具有固定大小、分配 V8 外堆的原始内存的特点。`Buffer` 的大小在创建时被确定，且不能调整大小。

`Buffer` 类在 Node.js 中是一个全局变量，因此不太可能像其他的模块那样永远需要使用 `require('buffer')`。

```javascript
const buf1 = Buffer.alloc(10);
// Creates a zero-filled Buffer of length 10.

const buf2 = Buffer.alloc(10, 1);
// Creates a Buffer of length 10, filled with 0x01.

const buf3 = Buffer.allocUnsafe(10);
// Creates an uninitialized buffer of length 10.
// This is faster than calling Buffer.alloc() but the returned
// Buffer instance might contain old data that needs to be
// overwritten using either fill() or write().

const buf4 = Buffer.from([1, 2, 3]);
// Creates a Buffer containing [01, 02, 03].

const buf5 = Buffer.from('test');
// Creates a Buffer containing ASCII bytes [74, 65, 73, 74].

const buf6 = Buffer.from('tést', 'utf8');
// Creates a Buffer containing UTF8 bytes [74, c3, a9, 73, 74].
```