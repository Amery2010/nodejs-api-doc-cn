# Buffer类

### 类相关的方法和属性

* [new Buffer(size)](#new_buffer_size)
* [new Buffer(array)](#new_buffer_array)
* [new Buffer(buffer)](#new_buffer_buffer)
* [new Buffer(arrayBuffer[, byteOffset[, length]])](#new_buffer_arrayBuffer)
* [new Buffer(str[, encoding])](#new_buffer_str)
* [Buffer.byteLength(string[, encoding])](#Buffer_byteLength)
* [Buffer.isBuffer(obj)](#Buffer_isBuffer)
* [Buffer.isEncoding(encoding)](#Buffer_isEncoding)
* [Buffer.from(array)](#Buffer_from_array)
* [Buffer.from(buffer)](#Buffer_from_buffer)
* [Buffer.from(arrayBuffer[, byteOffset[, length]])](#Buffer_from_arrayBuffer)
* [Buffer.from(str[, encoding])](#Buffer_from_str)
* [Buffer.alloc(size[, fill[, encoding]])](#Buffer_alloc)
* [Buffer.allocUnsafe(size)](#Buffer_allocUnsafe)
* [Buffer.concat(list[, totalLength])](#Buffer_concat)
* [Buffer.compare(buf1, buf2)](#Buffer_compare)

### 实例相关的方法和属性

* [buf.length](#length)
* [buf[index]](#index)
* [buf.toString([encoding[, start[, end]]])](#toString)
* [buf.toJSON()](#toJSON)
* [buf.slice([start[, end]])](#slice)
* [buf.fill(value[, offset[, end]][, encoding])](#fill)
* [buf.indexOf(value[, byteOffset][, encoding])](#indexOf)
* [buf.includes(value[, byteOffset][, encoding])](#includes)
* [buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])](#copy)
* [buf.entries()](#entries)
* [buf.values()](#values)
* [buf.compare(otherBuffer)](#compare)
* [buf.equals(otherBuffer)](#equals)
* [buf.readIntBE(offset, byteLength[, noAssert])](#readIntBE)
* [buf.readIntLE(offset, byteLength[, noAssert])](#readIntLE)
* [buf.readFloatBE(offset[, noAssert])](#readFloatBE)
* [buf.readFloatLE(offset[, noAssert])](#readFloatLE)
* [buf.readDoubleBE(offset[, noAssert])](#readDoubleBE)
* [buf.readDoubleLE(offset[, noAssert])](#readDoubleLE)
* [buf.readInt8(offset[, noAssert])](#readInt8)
* [buf.readInt16BE(offset[, noAssert])](#readInt16BE)
* [buf.readInt16LE(offset[, noAssert])](#readInt16LE)
* [buf.readInt32BE(offset[, noAssert])](#readInt32BE)
* [buf.readInt32LE(offset[, noAssert])](#readInt32LE)
* [buf.readUIntBE(offset, byteLength[, noAssert])](#readUIntBE)
* [buf.readUIntLE(offset, byteLength[, noAssert])](#readUIntLE)
* [buf.readUInt8(offset[, noAssert])](#readUInt8)
* [buf.readUInt16BE(offset[, noAssert])](#readUInt16BE)
* [buf.readUInt16LE(offset[, noAssert])](#readUInt16LE)
* [buf.readUInt32BE(offset[, noAssert])](#readUInt32BE)
* [buf.readUInt32LE(offset[, noAssert])](#readUInt32LE)
* [buf.write(string[, offset[, length]][, encoding])](#write)
* [buf.writeIntBE(value, offset, byteLength[, noAssert])](#writeIntBE)
* [buf.writeIntLE(value, offset, byteLength[, noAssert])](#writeIntLE)
* [buf.writeFloatBE(value, offset[, noAssert])](#writeFloatBE)
* [buf.writeFloatLE(value, offset[, noAssert])](#writeFloatLE)
* [buf.writeDoubleBE(value, offset[, noAssert])](#writeDoubleBE)
* [buf.writeDoubleLE(value, offset[, noAssert])](#writeDoubleLE)
* [buf.writeInt8(value, offset[, noAssert])](#writeInt8)
* [buf.writeInt16BE(value, offset[, noAssert])](#writeInt16BE)
* [buf.writeInt16LE(value, offset[, noAssert])](#writeInt16LE)
* [buf.writeInt32BE(value, offset[, noAssert])](#writeInt32BE)
* [buf.writeInt32LE(value, offset[, noAssert])](#writeInt32LE)
* [buf.writeUIntBE(value, offset, byteLength[, noAssert])](#writeUIntBE)
* [buf.writeUIntLE(value, offset, byteLength[, noAssert])](#writeUIntLE)
* [buf.writeUInt8(value, offset[, noAssert])](#writeUInt8)
* [buf.writeUInt16BE(value, offset[, noAssert])](#writeUInt16BE)
* [buf.writeUInt16LE(value, offset[, noAssert])](#writeUInt16LE)
* [buf.writeUInt32BE(value, offset[, noAssert])](#writeUInt32BE)
* [buf.writeUInt32LE(value, offset[, noAssert])](#writeUInt32LE)

--------------------------------------------------


Buffer 类是一个全局变量类型，用来直接处理2进制数据的。 它能够使用多种方式构建。


<div id="new_buffer_size" class="anchor"></div>
## new Buffer(size)

- `size` {Number}

分配一个 `size` 字节大小的新 `Buffer`。`size` 必须小于等于 `require('buffer').kMaxLength`（在64位架构上 `kMaxLength` 的大小是 `(2^31)-1`）的值，否则将抛出一个 [RangeError](../errors/class_RangeError.md#) 的错误。如果 `size` 小于 0 将创建一个特定的 0 长度（zero-length ）的 Buffer。

不像 `ArrayBuffers` ，以这种方式创建的 Buffer 实例的底层内存是*没被优化过*的。新创建的 Buffer 的内容是*未知*的，并*可能包含敏感数据*。通过使用 `buf.fill(0)` 将一个 Buffer 初始化为零。

```javascript
const buf = new Buffer(5);
console.log(buf);
// <Buffer 78 e0 82 02 01/>
// (octets will be different, every time)
buf.fill(0);
console.log(buf);
// <Buffer 00 00 00 00 00/>
```


<div id="new_buffer_array" class="anchor"></div>
## new Buffer(array)

- `array` {Array}

分配 8 位字节大小的 `array` 的一个新的 `Buffer` 。

```javascript
const buf = new Buffer([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);
// creates a new Buffer containing ASCII bytes
// ['b','u','f','f','e','r']
```


<div id="new_buffer_buffer" class="anchor"></div>
## new Buffer(buffer)

- `buffer` {Buffer}

将所传的 `buffer` 数据拷贝到新建的 `Buffer` 实例中。

```javascript
const buf1 = new Buffer('buffer');
const buf2 = new Buffer(buf1);

buf1[0] = 0x61;
console.log(buf1.toString());
// 'auffer'
console.log(buf2.toString());
// 'buffer' (copy is not changed)
```


<div id="new_buffer_arrayBuffer" class="anchor"></div>
## new Buffer(arrayBuffer[, byteOffset[, length]])

- `arrayBuffer` - 一个 `arrayBuffer` 或 `new ArrayBuffer()` 的 `.buffer` 属性

- `byteOffset` {Number} 默认：0

- `length` {Number} 默认：`arrayBuffer.length - byteOffset`

当传递的是 `TypedArray` 实例的 `.buffer` 引用时，这个新建的 Buffer 将像 `TypedArray` 那样共享相同的内存分配。

选填的 `byteOffset` 和 `length` 参数指定一个将由 Buffer 共享的 `arrayBuffer` 中的内存范围。

```javascript
const arr = new Uint16Array(2);
arr[0] = 5000;
arr[1] = 4000;

const buf = new Buffer(arr.buffer); // shares the memory with arr;

console.log(buf);
// Prints: <Buffer 88 13 a0 0f/>

// changing the TypdArray changes the Buffer also
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17/>
```


<div id="new_buffer_str" class="anchor"></div>
## new Buffer(str[, encoding])

- `str` {String} 需要编码的字符串

- `encoding` {String} 默认：`'utf8'`

创建一个新的 Buffer 包含给定的 JavaScript 字符串 `str`。如果提供 `encoding` 参数，将标识字符串的字符编码。

```javascript
const buf1 = new Buffer('this is a tést');
console.log(buf1.toString());
// prints: this is a tést
console.log(buf1.toString('ascii'));
// prints: this is a tC)st

const buf2 = new Buffer('7468697320697320612074c3a97374', 'hex');
console.log(buf2.toString());
// prints: this is a tést
```


<div id="Buffer_byteLength" class="anchor"></div>
## Buffer.byteLength(string[, encoding])

- `string` {String} | {Buffer} | {TypedArray} | {DataView} | {ArrayBuffer}

- `encoding` {String} 默认：`'utf8'`

- 返回：{Number}

返回一个字符串的实际字节长度。这与 [String.prototype.length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/length) 不同，因为它是返回字符串中的*字符*数目。

例如：

```javascript
const str = '\u00bd + \u00bc = \u00be';

console.log(`${str}: ${str.length} characters, ` +
            `${Buffer.byteLength(str, 'utf8')} bytes`);

// ½ + ¼ = ¾: 9 characters, 12 bytes
```

当 `string` 是一个 `Buffer` / [DataView](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DataView) / [TypedArray](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) / `ArrayBuffer` 时，返回实际的字节长度。

除此之外，将转换为 `String` 并返回字符串的字节长度。


<div id="Buffer_isBuffer" class="anchor"></div>
## Buffer.isBuffer(obj)

- `obj` {Object}

- 返回：{Boolean}

如果 `obj` 是一个 Buffer 则返回 `true`。


<div id="Buffer_isEncoding" class="anchor"></div>
## Buffer.isEncoding(encoding)

- `encoding` {String} 需要测试的编码字符串

- 返回：{Boolean}

如果 `encoding` 是一个有效的编码参数则返回 `true`，否则返回 `false`。


<div id="Buffer_from_array" class="anchor"></div>
## Buffer.from(array)

- `array` {Array}

使用一个8位字节的数组分配一个新的 Buffer。

```javascript
const buf = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);
// creates a new Buffer containing ASCII bytes
// ['b','u','f','f','e','r']
```

如果 `array` 不是一个有效的 `Array` 则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。


<div id="Buffer_from_buffer" class="anchor"></div>
## Buffer.from(buffer)

- `buffer` {Buffer}

将所传的 `buffer` 数据拷贝到这个新的 `Buffer` 实例中。

```javascript
const buf1 = Buffer.from('buffer');
const buf2 = Buffer.from(buf1);

buf1[0] = 0x61;
console.log(buf1.toString());
// 'auffer'
console.log(buf2.toString());
// 'buffer' (copy is not changed)
```

如果 `buffer` 不是一个有效的 `Buffer` 则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。


<div id="Buffer_from_arrayBuffer" class="anchor"></div>
## Buffer.from(arrayBuffer[, byteOffset[, length]])

- `arrayBuffer` - 一个 `TypedArray` 或 `new ArrayBuffer()` 的 `.buffer` 属性

- `byteOffset` {Number} 默认：0

- `length` {Number} 默认：`arrayBuffer.length - byteOffset`

当传递的是 `TypedArray` 实例的 `.buffer` 引用时，这个新建的 Buffer 将像 `TypedArray` 那样共享相同的内存分配。

```javascript
const arr = new Uint16Array(2);
arr[0] = 5000;
arr[1] = 4000;

const buf = Buffer.from(arr.buffer); // shares the memory with arr;

console.log(buf);
// Prints: <Buffer 88 13 a0 0f/>

// changing the TypedArray changes the Buffer also
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17/>
```

选填的 `byteOffset` 和 `length` 参数指定一个将由 `Buffer` 共享的 `arrayBuffer` 中的内存范围。

```javascript
const ab = new ArrayBuffer(10);
const buf = Buffer.from(ab, 0, 2);
console.log(buf.length);
// Prints: 2
```

如果 `arrayBuffer` 不是一个有效的 `ArrayBuffer` 则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。


<div id="Buffer_from_str" class="anchor"></div>
## Buffer.from(str[, encoding])

- `str` {String} 需要编码的字符串

- `encoding` {String} 编码时用到，默认：`'utf8'`

创建一个新的 Buffer 包含给定的 JavaScript 字符串 `str`。如果提供 `encoding` 参数，将标识字符串的字符编码。如果没有提供 `encoding` 参数，默认为 `'utf8'`。

```javascript
const buf1 = Buffer.from('this is a tést');
console.log(buf1.toString());
// prints: this is a tést
console.log(buf1.toString('ascii'));
// prints: this is a tC)st

const buf2 = Buffer.from('7468697320697320612074c3a97374', 'hex');
console.log(buf2.toString());
// prints: this is a tést
```

如果 `str` 不是一个有效的 `String` 则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。


<div id="Buffer_alloc" class="anchor"></div>
## Buffer.alloc(size[, fill[, encoding]])

- `size` {Number}

- `fill` {Value} 默认：`undefined`

- `encoding` {String} 默认：`utf8`

分配一个 `size` 字节大小的新 `Buffer`。如果 `fill` 是 `undefined` ，该 `Buffer` 将被零填充（zero-filled）。

```javascript
const buf = Buffer.alloc(5);
console.log(buf);
// <Buffer 00 00 00 00 00/>
```

`size` 必须小于等于 `require('buffer').kMaxLength`（在64位架构上 `kMaxLength` 的大小是 `(2^31)-1`）的值，否则将抛出一个 [RangeError](../errors/class_RangeError.md#) 的错误。如果 `size` 小于 0 将创建一个特定的 0 长度（zero-length ）的 Buffer。

如果指定了 `fill` 参数，将通过调用 [buf.fill(fill)](#fill) 初始化当前 `Buffer` 的分配。

```javascript
const buf = Buffer.alloc(5, 'a');
console.log(buf);
// <Buffer 61 61 61 61 61/>
```

如果同时指定了 `fill` 和 `encoding` 参数，将通过调用 [buf.fill(fill, encoding)](#fill) 初始化当前 `Buffer` 的分配。例如：

```javascript
const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');
console.log(buf);
// <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64/>
```

调用 `Buffer.alloc(size)` 方法显然要比替代的 `Buffer.allocUnsafe(size)` 要慢，但可以确保新建的 Buffer 实例的内容*不会包含敏感数据*。

如果 `size` 不是一个数字则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。


<div id="Buffer_allocUnsafe" class="anchor"></div>
## Buffer.allocUnsafe(size)

- `size` {Number}

分配一个 `size` 字节大小的新的*非零填充（non-zero-filled）*的 `Buffer`。`size` 必须小于等于 `require('buffer').kMaxLength`（在64位架构上 `kMaxLength` 的大小是 `(2^31)-1`）的值，否则将抛出一个 [RangeError](../errors/class_RangeError.md#) 的错误。如果 `size` 小于 0 将创建一个特定的 0 长度（zero-length ）的 Buffer。

以这种方式创建的 Buffer 实例的底层内存是*没被优化过*的。新创建的 Buffer 的内容是*未知*的，并*可能包含敏感数据*。通过使用 `buf.fill(0)` 将这个 `Buffer` 初始化为零。

```javascript
const buf = Buffer.allocUnsafe(5);
console.log(buf);
// <Buffer 78 e0 82 02 01/>
// (octets will be different, every time)
buf.fill(0);
console.log(buf);
// <Buffer 00 00 00 00 00/>
```

如果 `size` 不是一个数字则抛出一个 [TypeError](../errors/class_TypeError.md#) 错误。

请注意，`Buffer` 模块预分配一个大小为 `Buffer.poolSize` 的内部 `Buffer` 实例作为一个快速分配池，仅当 `size` 小于等于 `Buffer.poolSize >> 1` （浮点类型的 `Buffer.poolSize` 应该除以2）时，用于分配通过 `Buffer.allocUnsafe(size)` 创建的新的 `Buffer` 实例（和 `new Buffer(size)` 构造函数）。默认的 `Buffer.poolSize` 值为 `8192` ，但可以被修改。

使用这个预分配的内部内存池是调用 `Buffer.alloc(size, fill)` 和 `Buffer.allocUnsafe(size).fill(fill)` 的关键不同之处。特别是，如果 `size` 小于或等于 `Buffer.poolSize` 的一半时，`Buffer.allocUnsafe(size).fill(fill)` *将*使用内部的 Buffer 池，而 `Buffer.alloc(size, fill)` 将*绝不会*使用这个内部的 Buffer 池。在一个应用程序需要 `Buffer.allocUnsafe(size)` 提供额外的性能时，（了解）这个细微的不同之处是非常重要的。


<div id="Buffer_concat" class="anchor"></div>
## Buffer.concat(list[, totalLength])

- `list` {Array} 需要连接的 Buffer 对象数组

- `totalLength` {Number} 上述需要被连接的 Buffer 的总大小。

- 返回：{Buffer}

返回一个连接了 `list` 中所有 Buffer 的新 Buffer 。

如果 `list` 中没有项目，或者当 `totalLength` 为 0 时，将返回一个 0 长度（zero-length）的 Buffer 。

如果没有提供 `totalLength` ，它将计算 `list` 中的 Buffer（以获得该值）。然而，这增加了额外的函数循环，提供精准的长度将加速计算。

例如：将一个包含三个 Buffer 的数组构建为一个单一的 Buffer ：

```javascript
const buf1 = Buffer.alloc(10, 0);
const buf2 = Buffer.alloc(14, 0);
const buf3 = Buffer.alloc(18, 0);
const totalLength = buf1.length + buf2.length + buf3.length;

console.log(totalLength);
const bufA = Buffer.concat([buf1, buf2, buf3], totalLength);
console.log(bufA);
console.log(bufA.length);

// 42
// <Buffer 00 00 00 00 ... />
// 42
```


<div id="Buffer_compare" class="anchor"></div>
## Buffer.compare(buf1, buf2)

- `buf1` {Buffer}

- `buf2` {Buffer}

- 返回：{Number}

比较 `buf1` 和 `buf2` 通常用于 Buffer 数组的排序目的。这相当于是调用 [buf1.compare(buf2)](#compare) 。

```javascript
const arr = [Buffer.from('1234'), Buffer.from('0123')];
arr.sort(Buffer.compare);
```

