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
* [buf.compare(otherBuffer)](#compare)
* [buf.equals(otherBuffer)](#equals)
* [buf.entries()](#entries)
* [buf.keys()](#keys)
* [buf.values()](#values)
* [buf.swap16()](#swap16)
* [buf.swap32()](#swap32)
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
// <Buffer 78 e0 82 02 01>
// (octets will be different, every time)
buf.fill(0);
console.log(buf);
// <Buffer 00 00 00 00 00>
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
// Prints: <Buffer 88 13 a0 0f>

// changing the TypdArray changes the Buffer also
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17>
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
// Prints: <Buffer 88 13 a0 0f>

// changing the TypedArray changes the Buffer also
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17>
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
// <Buffer 00 00 00 00 00>
```

`size` 必须小于等于 `require('buffer').kMaxLength`（在64位架构上 `kMaxLength` 的大小是 `(2^31)-1`）的值，否则将抛出一个 [RangeError](../errors/class_RangeError.md#) 的错误。如果 `size` 小于 0 将创建一个特定的 0 长度（zero-length ）的 Buffer。

如果指定了 `fill` 参数，将通过调用 [buf.fill(fill)](#fill) 初始化当前 `Buffer` 的分配。

```javascript
const buf = Buffer.alloc(5, 'a');
console.log(buf);
// <Buffer 61 61 61 61 61>
```

如果同时指定了 `fill` 和 `encoding` 参数，将通过调用 [buf.fill(fill, encoding)](#fill) 初始化当前 `Buffer` 的分配。例如：

```javascript
const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');
console.log(buf);
// <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
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
// <Buffer 78 e0 82 02 01>
// (octets will be different, every time)
buf.fill(0);
console.log(buf);
// <Buffer 00 00 00 00 00>
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
// <Buffer 00 00 00 00 ... >
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


--------------------------------------------------

## 实例的属性和方法


<div id="length" class="anchor"></div>
#### buf.length

- {Number}

返回该 Buffer 在字节数上分配的内存量。注意，这并不一定反映 Buffer 内可用的数据量。例如在下面的例子中，一个 Buffer 分配了 1234 字节，但只写入了 11 ASCII 字节。

```javascript
const buf = Buffer.allocUnsafe(1234);

console.log(buf.length);
// Prints: 1234

buf.write('some string', 0, 'ascii');
console.log(buf.length);
// Prints: 1234
```

而 `length` 属性并非一成不变，改变 `length` 值会导致不确定和不一致的行为。应用程序如果希望修改一个 Buffer 的长度，因而需要把 `length` 设为只读并使用 [buf.slice()](#slice) 创建一个新的 Buffer 。

```javascript
var buf = Buffer.allocUnsafe(10);
buf.write('abcdefghj', 0, 'ascii');
console.log(buf.length);
// Prints: 10
buf = buf.slice(0, 5);
console.log(buf.length);
// Prints: 5
```


<div id="index" class="anchor"></div>
#### buf[index]

索引操作符 `[index]` 可用于获取或设置 Buffer 中指定 `index` 位置的8位字节。这个值指的是单个字节，所以这个值合法范围是16进制的 0x00 到 0xFF，或 10进制的 0 到 255。

例如，将一个 ASCII 字符串拷贝到一个 Buffer 中，一次一个字节：

```javascript
const str = "Node.js";
const buf = Buffer.allocUnsafe(str.length);

for (var i = 0; i < str.length; i++) {
    buf[i] = str.charCodeAt(i);
}

console.log(buf.toString('ascii'));
// Prints: Node.js
```


<div id="toString" class="anchor"></div>
#### buf.toString([encoding[, start[, end]]])

- `encoding` {String} 默认：`'utf8'`

- `start` {Number} 默认：0

- `end` {Number} 默认：`buffer.length`

- 返回：{String}

返回使用指定的字符集编码解码 Buffer 数据的字符串。

```javascript
const buf = Buffer.allocUnsafe(26);
for (var i = 0; i < 26; i++) {
    buf[i] = i + 97; // 97 is ASCII a
}
buf.toString('ascii');
// Returns: 'abcdefghijklmnopqrstuvwxyz'
buf.toString('ascii', 0, 5);
// Returns: 'abcde'
buf.toString('utf8', 0, 5);
// Returns: 'abcde'
buf.toString(undefined, 0, 5);
// Returns: 'abcde', encoding defaults to 'utf8'
```


<div id="toJSON" class="anchor"></div>
#### buf.toJSON()

- 返回：{Object}

返回该 Buffer 实例的 JSON 表达式。当字符串化一个 Buffer 实例时会隐式调用 [JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 这个函数。

例子：

```javascript
const buf = Buffer.from('test');
const json = JSON.stringify(buf);

console.log(json);
// Prints: '{"type":"Buffer","data":[116,101,115,116]}'

const copy = JSON.parse(json, (key, value) => {
    return value && value.type === 'Buffer' ? Buffer.from(value.data) : value;
});

console.log(copy.toString());
// Prints: 'test'
```


<div id="slice" class="anchor"></div>
#### buf.slice([start[, end]])

- `start` {Number} 默认：0

- `end` {Number} 默认：`buffer.length`

- 返回：{Buffer}

返回一个指向相同原始内存的新 Buffer ，但会有偏移并通过 `start` 和 `end` 索引值进行裁剪。

**请注意，修改这个新的 Buffer 切片，将会修改原始 Buffer 的内存，因为这两个对象共享所分配的内存。**

例子：创建一个 ASCII 字母的 Buffer，进行切片，然后修改原始 Buffer 上的一个字节。

```javascript
const buf1 = Buffer.allocUnsafe(26);

for (var i = 0; i < 26; i++) {
    buf1[i] = i + 97; // 97 is ASCII a
}

const buf2 = buf1.slice(0, 3);
buf2.toString('ascii', 0, buf2.length);
// Returns: 'abc'
buf1[0] = 33;
buf2.toString('ascii', 0, buf2.length);
// Returns : '!bc'
```

指定负索引会导致产生相对于这个 Buffer 的末尾而不是开头的切片（slice）。

```javascript
const buf = Buffer.from('buffer');

buf.slice(-6, -1).toString();
// Returns 'buffe', equivalent to buf.slice(0, 5)
buf.slice(-6, -2).toString();
// Returns 'buff', equivalent to buf.slice(0, 4)
buf.slice(-5, -2).toString();
// Returns 'uff', equivalent to buf.slice(1, 4)
```


<div id="fill" class="anchor"></div>
#### buf.fill(value[, offset[, end]][, encoding])

- `value` {String} | {Buffer} | {Number}

- `offset` {Number} 默认：0

- `end` {Number} 默认：`buf.length`

- `encoding` {String} 默认：`'utf8'`

- 返回：{Buffer}

使用指定的值填充当前 Buffer 。如果 `offset` (默认是 `0`) 和 `end` (默认是 `buffer.length`) 没有明确给出，将会填充整个 buffer 。该方法返回一个当前 Buffer 的引用，以便于链式调用。这也意味着可以通过这种小而简的方式创建一个 Buffer 。允许在单行内创建和填充 Buffer ：

```javascript
const b = Buffer.alloc(50, 'h');
console.log(b.toString());
// Prints: hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
```

`encoding` 只在 `value` 是一个字符串时应用，除此之外，都会被忽略。如果 `value` 不是一个 `String` 或 `Number` ，则会被强制转换到 `uint32` 类型。

`fill()` 操作默默地向 Buffer 里写入字节。即便最终写入落在多字节字符之间，它也会将这些字节塞到被写入的 buffer 里。

```javascript
Buffer.alloc(3, '\u0222');
// Prints: <Buffer c8 a2 c8>
```


<div id="indexOf" class="anchor"></div>
#### buf.indexOf(value[, byteOffset][, encoding])

- `value` {String} | {Buffer} | {Number}

- `byteOffset` {Number} 默认：0

- `encoding` {String} 默认：`'utf8'`

- 返回：{Number}

该操作类似于 [Array#indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) ，它返回 `value` 在 Buffer 中的最开始的索引位置，如果当前 Buffer 不包含这个 `value` 则返回 `-1` 。这个 `value` 的值可以是 `String` 、`Buffer` 或 `Number` 。字符串会默认用 UTF8 解释执行。Buffer 将会使用整个 Buffer（比较部分 Buffer 请使用 [buf.slice()](#slice) 方法）。数字在 0 到 255 的范围内。 

```javascript
const buf = Buffer.from('this is a buffer');

buf.indexOf('this');
// returns 0
buf.indexOf('is');
// returns 2
buf.indexOf(Buffer.from('a buffer'));
// returns 8
buf.indexOf(97); // ascii for 'a'
// returns 8
buf.indexOf(Buffer.from('a buffer example'));
// returns -1
buf.indexOf(Buffer.from('a buffer example').slice(0, 8));
// returns 8

const utf16Buffer = Buffer.from('\u039a\u0391\u03a3\u03a3\u0395', 'ucs2');

utf16Buffer.indexOf('\u03a3', 0, 'ucs2');
// returns 4
utf16Buffer.indexOf('\u03a3', -4, 'ucs2');
// returns 6
```


<div id="includes" class="anchor"></div>
#### buf.includes(value[, byteOffset][, encoding])


- `value` {String} | {Buffer} | {Number}

- `byteOffset` {Number} 默认：0

- `encoding` {String} 默认：`'utf8'`

- 返回：{Boolean}

该操作类似于 [Array#includes()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)。这个 `value` 的值可以是 `String` 、`Buffer` 或 `Number` 。字符串会被作为 UTF8 解释执行，除非你覆盖了 `encoding` 参数。Buffer 将会使用整个 Buffer（比较部分 Buffer 请使用 [buf.slice()](#slice) 方法）。数字在 0 到 255 的范围内。 

`byteOffset` 表示在搜索 `buf` 时的初始索引值。

```javascript
const buf = Buffer.from('this is a buffer');

buf.includes('this');
// returns true
buf.includes('is');
// returns true
buf.includes(Buffer.from('a buffer'));
// returns true
buf.includes(97); // ascii for 'a'
// returns true
buf.includes(Buffer.from('a buffer example'));
// returns false
buf.includes(Buffer.from('a buffer example').slice(0, 8));
// returns true
buf.includes('this', 4);
// returns false
```


<div id="copy" class="anchor"></div>
#### buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])

- `targetBuffer` {Buffer} 需要拷贝的 Buffer

- `targetStart` {Number} 默认：0

- `sourceStart` {Number} 默认：0

- `sourceEnd` {Number} 默认：`buffer.length`

- 返回：{Number} 被拷贝的字节数

将这个 Buffer 的一个区域的数据拷贝到目标 Buffer 的一个区域，即便与目标是共享内存区域资源的。

例子：创建两个 Buffer ，然后把将 `buf1` 第 16 到 19 位的字节拷贝到 `buf2` 中，并从 `buf2` 的第 8 位开始（覆盖）。

```javascript
const buf1 = Buffer.allocUnsafe(26);
const buf2 = Buffer.allocUnsafe(26).fill('!');

for (var i = 0; i < 26; i++) {
    buf1[i] = i + 97; // 97 is ASCII a
}

buf1.copy(buf2, 8, 16, 20);
console.log(buf2.toString('ascii', 0, 25));
// Prints: !!!!!!!!qrst!!!!!!!!!!!!!
```

例子：创建一个单一的 Buffer ，然后将一块区域的数据拷贝到同一个 Buffer 中另一块交叉的区域。

```javascript
const buf = Buffer.allocUnsafe(26);

for (var i = 0; i < 26; i++) {
    buf[i] = i + 97; // 97 is ASCII a
}

buf.copy(buf, 0, 4, 10);
console.log(buf.toString());

// efghijghijklmnopqrstuvwxyz
```


<div id="compare" class="anchor"></div>
#### buf.compare(otherBuffer)

- `otherBuffer` {Buffer}

- 返回：{Number}

比较两个 Buffer 实例，无论 `buf` 在排序上靠前、靠后甚至与 `otherBuffer` 相同都会返回一个数字标识。比较是基于在每个 Buffer 的实际字节序列。

* 如果 `otherBuffer` 和 `buf` 相同，则返回 `0`

* 如果 `otherBuffer` 排在 `buf` *前面*，则返回 `1`

* 如果 `otherBuffer` 排在 `buf` *后面*，则返回 `-1`

```javascript
const buf1 = Buffer.from('ABC');
const buf2 = Buffer.from('BCD');
const buf3 = Buffer.from('ABCD');

console.log(buf1.compare(buf1));
// Prints: 0
console.log(buf1.compare(buf2));
// Prints: -1
console.log(buf1.compare(buf3));
// Prints: 1
console.log(buf2.compare(buf1));
// Prints: 1
console.log(buf2.compare(buf3));
// Prints: 1

[buf1, buf2, buf3].sort(Buffer.compare);
// produces sort order [buf1, buf3, buf2]
```


<div id="equals" class="anchor"></div>
#### buf.equals(otherBuffer)

- `otherBuffer` {Buffer}

- 返回：{Boolean}

返回一个 boolean 标识，无论 `this` 和 `otherBuffer` 是否具有完全相同的字节。

```javascript
const buf1 = Buffer.from('ABC');
const buf2 = Buffer.from('414243', 'hex');
const buf3 = Buffer.from('ABCD');

console.log(buf1.equals(buf2));
// Prints: true
console.log(buf1.equals(buf3));
// Prints: false
```


<div id="entries" class="anchor"></div>
#### buf.entries()

- 返回：{Iterator}

从当前 Buffer 的内容中，创建并返回一个 `[index, byte]` 形式的[迭代器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)。

```javascript
const buf = Buffer.from('buffer');
for (var pair of buf.entries()) {
    console.log(pair);
}
// prints:
//   [0, 98]
//   [1, 117]
//   [2, 102]
//   [3, 102]
//   [4, 101]
//   [5, 114]
```


<div id="keys" class="anchor"></div>
#### buf.keys()

- 返回：{Iterator}

创建并返回一个包含 Buffer 键名（索引）的[迭代器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)。

```javascript
const buf = Buffer.from('buffer');
for (var key of buf.keys()) {
    console.log(key);
}
// prints:
//   0
//   1
//   2
//   3
//   4
//   5
```


<div id="values" class="anchor"></div>
#### buf.values()

- 返回：{Iterator}

创建并返回一个包含 Buffer 值（字节）的[迭代器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)。当 Buffer 使用 `for..of` 声明时将自动调用该函数。

```javascript
const buf = Buffer.from('buffer');
for (var value of buf.values()) {
    console.log(value);
}
// prints:
//   98
//   117
//   102
//   102
//   101
//   114

for (var value of buf) {
    console.log(value);
}
// prints:
//   98
//   117
//   102
//   102
//   101
//   114
```


<div id="swap16" class="anchor"></div>
#### buf.swap16()

- 返回：{Buffer}

将 Buffer 解释执行为一个16位的无符号整数数组并以字节顺序交换到位。如果 Buffer 的长度不是16位的倍数，则抛出一个 [RangeError](../errors/class_RangeError.md#) 错误。该方法返回一个当前 Buffer 的引用，以便于链式调用。

```javascript
const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf);
// Prints Buffer(0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8)
buf.swap16();
console.log(buf);
// Prints Buffer(0x2, 0x1, 0x4, 0x3, 0x6, 0x5, 0x8, 0x7)
```


<div id="swap32" class="anchor"></div>
#### buf.swap32()

- 返回：{Buffer}

将 Buffer 解释执行为一个32位的无符号整数数组并以字节顺序交换到位。如果 Buffer 的长度不是32位的倍数，则抛出一个 [RangeError](../errors/class_RangeError.md#) 错误。该方法返回一个当前 Buffer 的引用，以便于链式调用。

```javascript
const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf);
// Prints Buffer(0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8)
buf.swap32();
console.log(buf);
// Prints Buffer(0x4, 0x3, 0x2, 0x1, 0x8, 0x7, 0x6, 0x5)
```


<div id="readIntBE" class="anchor"></div>
#### buf.readIntBE(offset, byteLength[, noAssert])
<div id="readIntLE" class="anchor"></div>
#### buf.readIntLE(offset, byteLength[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - byteLength`

- `byteLength` {Number} `0 < byteLength <= 6`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的 `offset` 位置开始读取 `byteLength` 字节数，并将结果解释执行为一个有符号的2的补码值。支持多达48位精度的值。例如：

```javascript
const buf = Buffer.allocUnsafe(6);
buf.writeUInt16LE(0x90ab, 0);
buf.writeUInt32LE(0x12345678, 2);
buf.readIntLE(0, 6).toString(16); // Specify 6 bytes (48 bits)
// Returns: '1234567890ab'

buf.readIntBE(0, 6).toString(16);
// Returns: -546f87a9cbee
```

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。


<div id="readFloatBE" class="anchor"></div>
#### buf.readFloatBE(offset[, noAssert])
<div id="readFloatLE" class="anchor"></div>
#### buf.readFloatLE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readFloatBE()` 返回一个较大的尾数，`readFloatLE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个32位浮点值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

```javascript
const buf = Buffer.from([1, 2, 3, 4]);

buf.readFloatBE();
// Returns: 2.387939260590663e-38
buf.readFloatLE();
// Returns: 1.539989614439558e-36
buf.readFloatLE(1);
// throws RangeError: Index out of range

buf.readFloatLE(1, true); // Warning: reads passed end of buffer!
// Segmentation fault! don't do this!
```


<div id="readDoubleBE" class="anchor"></div>
#### buf.readDoubleBE(offset[, noAssert])
<div id="readDoubleLE" class="anchor"></div>
#### buf.readDoubleLE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 8`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readDoubleBE()` 返回一个较大的尾数，`readDoubleLE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个64位双精度值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

```javascript
const buf = Buffer.from([1, 2, 3, 4, 5, 6, 7, 8]);

buf.readDoubleBE();
// Returns: 8.20788039913184e-304
buf.readDoubleLE();
// Returns: 5.447603722011605e-270
buf.readDoubleLE(1);
// throws RangeError: Index out of range

buf.readDoubleLE(1, true); // Warning: reads passed end of buffer!
// Segmentation fault! don't do this!
```


<div id="readInt8" class="anchor"></div>
#### buf.readInt8(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 1`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的 `offset` 位置开始读取一个有符号的8位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

从 Buffer 里读取的整数数值会被解释执行为有符号的2的补码值。

```javascript
const buf = Buffer.from([1, -2, 3, 4]);

buf.readInt8(0);
// returns 1
buf.readInt8(1);
// returns -2
```


<div id="readInt16BE" class="anchor"></div>
#### buf.readInt16BE(offset[, noAssert])
<div id="readInt16LE" class="anchor"></div>
#### buf.readInt16LE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 2`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readInt16BE()` 返回一个较大的尾数，`readInt16LE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个16位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

从 Buffer 里读取的整数数值会被解释执行为有符号的2的补码值。

```javascript
const buf = Buffer.from([1, -2, 3, 4]);

buf.readInt16BE();
// returns 510
buf.readInt16LE(1);
// returns 1022
```


<div id="readInt32BE" class="anchor"></div>
#### buf.readInt32BE(offset[, noAssert])
<div id="readInt32LE" class="anchor"></div>
#### buf.readInt32LE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readInt32BE()` 返回一个较大的尾数，`readInt32LE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个有符号的32位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

从 Buffer 里读取的整数数值会被解释执行为有符号的2的补码值。

```javascript
const buf = Buffer.from([1, -2, 3, 4]);

buf.readInt32BE();
// returns 33424132
buf.readInt32LE();
// returns 67370497
buf.readInt32LE(1);
// throws RangeError: Index out of range
```


<div id="readUIntBE" class="anchor"></div>
#### buf.readUIntBE(offset, byteLength[, noAssert])
<div id="readUIntLE" class="anchor"></div>
#### buf.readUIntLE(offset, byteLength[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - byteLength`

- `byteLength` {Number} `0 < byteLength <= 6`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的 `offset` 位置开始读取 `byteLength` 字节数，并将结果解释执行为一个无符号的整数值。支持多达48位精度的值。例如：

```javascript
const buf = Buffer.allocUnsafe(6);
buf.writeUInt16LE(0x90ab, 0);
buf.writeUInt32LE(0x12345678, 2);
buf.readUIntLE(0, 6).toString(16); // Specify 6 bytes (48 bits)
// Returns: '1234567890ab'

buf.readUIntBE(0, 6).toString(16);
// Returns: ab9078563412
```

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。


<div id="readUInt8" class="anchor"></div>
#### buf.readUInt8(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 1`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的 `offset` 位置开始读取一个无符号的8位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

```javascript
const buf = Buffer.from([1, -2, 3, 4]);

buf.readUInt8(0);
// returns 1
buf.readUInt8(1);
// returns 254
```


<div id="readUInt16BE" class="anchor"></div>
#### buf.readUInt16BE(offset[, noAssert])
<div id="readUInt16LE" class="anchor"></div>
#### buf.readUInt16LE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 2`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readUInt16BE()` 返回一个较大的尾数，`readUInt16LE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个无符号的16位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

例子：

```javascript
const buf = Buffer.from([0x3, 0x4, 0x23, 0x42]);

buf.readUInt16BE(0);
// Returns: 0x0304
buf.readUInt16LE(0);
// Returns: 0x0403
buf.readUInt16BE(1);
// Returns: 0x0423
buf.readUInt16LE(1);
// Returns: 0x2304
buf.readUInt16BE(2);
// Returns: 0x2342
buf.readUInt16LE(2);
// Returns: 0x4223
```


<div id="readUInt32BE" class="anchor"></div>
#### buf.readUInt32BE(offset[, noAssert])
<div id="readUInt32LE" class="anchor"></div>
#### buf.readUInt32LE(offset[, noAssert])

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number}

从该 Buffer 指定的带有特定尾数格式（`readUInt32BE()` 返回一个较大的尾数，`readUInt32LE()` 返回一个较小的尾数）的 `offset` 位置开始读取一个无符号的32位整数值。

设置 `noAssert` 为 `true` ，将跳过对 `offset` 的验证。这将允许 `offset` 超出缓冲区的末尾。

例子：

```javascript
const buf = Buffer.from([0x3, 0x4, 0x23, 0x42]);

buf.readUInt32BE(0);
// Returns: 0x03042342
console.log(buf.readUInt32LE(0));
// Returns: 0x42230403
```


<div id="write" class="anchor"></div>
#### buf.write(string[, offset[, length]][, encoding])

- `string` {String} 需要被写入到 Buffer 的字节

- `offset` {Number} 默认：0

- `length` {Number} 默认：`buffer.length - offset`

- `encoding` {String} 默认：`'utf8'`

- 返回：{Number} 被写入的字节数

在 Buffer 的 `offset` 位置使用给定的 `encoding` 写入 `string` 。`length` 参数是写入的字节数。如果 Buffer 没有足够的空间以适应整个字符串，只会写入字符串的一部分，然而，它不会只写入已编码的字符部分。

```javascript
const buf = Buffer.allocUnsafe(256);
const len = buf.write('\u00bd + \u00bc = \u00be', 0);
console.log(`${len} bytes: ${buf.toString('utf8', 0, len)}`);
// Prints: 12 bytes: ½ + ¼ = ¾
```


<div id="writeIntBE" class="anchor"></div>
#### buf.writeIntBE(value, offset, byteLength[, noAssert])
<div id="writeIntLE" class="anchor"></div>
#### buf.writeIntLE(value, offset, byteLength[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - byteLength`

- `byteLength` {Number} 默认：`0 < byteLength <= 6`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

通过指定的 `offset` 和 `byteLength` 将 `value` 写入到当前 Buffer 中。支持多达 48 位的精度。例如：

```javascript
const buf1 = Buffer.allocUnsafe(6);
buf1.writeUIntBE(0x1234567890ab, 0, 6);
console.log(buf1);
// Prints: <Buffer 12 34 56 78 90 ab>

const buf2 = Buffer.allocUnsafe(6);
buf2.writeUIntLE(0x1234567890ab, 0, 6);
console.log(buf2);
// Prints: <Buffer ab 90 78 56 34 12>
```

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

当值不是一个整数时，它的行为是不确定的。


<div id="writeFloatBE" class="anchor"></div>
#### buf.writeFloatBE(value, offset[, noAssert])
<div id="writeFloatLE" class="anchor"></div>
#### buf.writeFloatLE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeFloatBE()` 写入一个较大的尾数，`writeFloatLE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。当值不是一个32位浮点值时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

例子：

```javascript
const buf = Buffer.allocUnsafe(4);
buf.writeFloatBE(0xcafebabe, 0);

console.log(buf);
// Prints: <Buffer 4f 4a fe bb>

buf.writeFloatLE(0xcafebabe, 0);

console.log(buf);
// Prints: <Buffer bb fe 4a 4f>
```


<div id="writeDoubleBE" class="anchor"></div>
#### buf.writeDoubleBE(value, offset[, noAssert])
<div id="writeDoubleLE" class="anchor"></div>
#### buf.writeDoubleLE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 8`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeDoubleBE()` 写入一个较大的尾数，`writeDoubleLE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。`value` 参数应当是一个有效的64位双精度值。当值不是一个64位双精度值时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

例子：

```javascript
const buf = Buffer.allocUnsafe(8);
buf.writeDoubleBE(0xdeadbeefcafebabe, 0);

console.log(buf);
// Prints: <Buffer 43 eb d5 b7 dd f9 5f d7>

buf.writeDoubleLE(0xdeadbeefcafebabe, 0);

console.log(buf);
// Prints: <Buffer d7 5f f9 dd b7 d5 eb 43>
```


<div id="writeInt8" class="anchor"></div>
#### buf.writeInt8(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 1`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

通过指定的 `offset` 将 `value` 写入到当前 Buffer 中。这个 `value` 应当是一个有效的有符号的8位整数。当值不是一个有符号的8位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

这个 `value` 作为一个2的补码的有符号的整数被解释执行和写入。

```javascript
const buf = Buffer.allocUnsafe(2);
buf.writeInt8(2, 0);
buf.writeInt8(-2, 1);
console.log(buf);
// Prints: <Buffer 02 fe>
```


<div id="writeInt16BE" class="anchor"></div>
#### buf.writeInt16BE(value, offset[, noAssert])
<div id="writeInt16LE" class="anchor"></div>
#### buf.writeInt16LE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 2`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeInt16BE()` 写入一个较大的尾数，`writeInt16LE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。`value` 参数应当是一个有效的有符号的16位整数。当值不是一个有符号的16位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

这个 `value` 作为一个2的补码的有符号的整数被解释执行和写入。

```javascript
const buf = Buffer.allocUnsafe(4);
buf.writeInt16BE(0x0102, 0);
buf.writeInt16LE(0x0304, 2);
console.log(buf);
// Prints: <Buffer 01 02 04 03>
```


<div id="writeInt32BE" class="anchor"></div>
#### buf.writeInt32BE(value, offset[, noAssert])
<div id="writeInt32LE" class="anchor"></div>
#### buf.writeInt32LE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeInt32BE()` 写入一个较大的尾数，`writeInt32LE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。`value` 参数应当是一个有效的有符号的32位整数。当值不是一个有符号的32位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

这个 `value` 作为一个2的补码的有符号的整数被解释执行和写入。

```javascript
const buf = Buffer.allocUnsafe(8);
buf.writeInt32BE(0x01020304, 0);
buf.writeInt32LE(0x05060708, 4);
console.log(buf);
// Prints: <Buffer 01 02 03 04 08 07 06 05>
```


<div id="writeUIntBE" class="anchor"></div>
#### buf.writeUIntBE(value, offset, byteLength[, noAssert])
<div id="writeUIntLE" class="anchor"></div>
#### buf.writeUIntLE(value, offset, byteLength[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - byteLength`

- `byteLength` {Number} 默认：`0 < byteLength <= 6`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

通过指定的 `offset` 和 `byteLength` 将 `value` 写入到当前 Buffer 中。支持多达 48 位的精度。例如：

```javascript
const buf = Buffer.allocUnsafe(6);
buf.writeUIntBE(0x1234567890ab, 0, 6);
console.log(buf);
// Prints: <Buffer 12 34 56 78 90 ab>
```

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

当值不是一个无符号的整数时，它的行为是不确定的。


<div id="writeUInt8" class="anchor"></div>
#### buf.writeUInt8(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 1`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

通过指定的 `offset` 将 `value` 写入到当前 Buffer 中。这个 `value` 应当是一个有效的无符号的8位整数。当值不是一个无符号的8位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

```javascript
const buf = Buffer.allocUnsafe(4);
buf.writeUInt8(0x3, 0);
buf.writeUInt8(0x4, 1);
buf.writeUInt8(0x23, 2);
buf.writeUInt8(0x42, 3);

console.log(buf);
// Prints: <Buffer 03 04 23 42>
```


<div id="writeUInt16BE" class="anchor"></div>
#### buf.writeUInt16BE(value, offset[, noAssert])
<div id="writeUInt16LE" class="anchor"></div>
#### buf.writeUInt16LE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 2`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeUInt16BE()` 写入一个较大的尾数，`writeUInt16LE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。`value` 参数应当是一个有效的无符号的16位整数。当值不是一个无符号的16位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

```javascript
const buf = Buffer.allocUnsafe(4);
buf.writeUInt16BE(0xdead, 0);
buf.writeUInt16BE(0xbeef, 2);

console.log(buf);
// Prints: <Buffer de ad be ef>

buf.writeUInt16LE(0xdead, 0);
buf.writeUInt16LE(0xbeef, 2);

console.log(buf);
// Prints: <Buffer ad de ef be>
```


<div id="writeUInt32BE" class="anchor"></div>
#### buf.writeUInt32BE(value, offset[, noAssert])
<div id="writeUInt32LE" class="anchor"></div>
#### buf.writeUInt32LE(value, offset[, noAssert])

- `value` {Number} 需要被写入到 Buffer 的字节

- `offset` {Number} `0 <= offset <= buf.length - 4`

- `noAssert` {Boolean} 默认：`false`

- 返回：{Number} 偏移加上被写入的字节数

从该 Buffer 指定的带有特定尾数格式（`writeUInt32BE()` 写入一个较大的尾数，`writeUInt32LE()` 写入一个较小的尾数）的 `offset` 位置开始写入 `value` 。`value` 参数应当是一个有效的无符号的32位整数。当值不是一个无符号的32位整数时，它的行为是不确定的。

将 `noAssert` 设为 `true` 将跳过对 `value` 和 `offset` 的验证。这意味着 `value` 可能对于这个特定的函数来说过大，并且 `offset` 可能超出该 Buffer 的末端，导致该值被直接丢弃。除非确定你的内容的正确性否则不应该被使用。

```javascript
const buf = Buffer.allocUnsafe(4);
buf.writeUInt32BE(0xfeedface, 0);

console.log(buf);
// Prints: <Buffer fe ed fa ce>

buf.writeUInt32LE(0xfeedface, 0);

console.log(buf);
// Prints: <Buffer ce fa ed fe>
```