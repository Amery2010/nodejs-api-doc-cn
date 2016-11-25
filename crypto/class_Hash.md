# Hash类

* [hash.update(data[, input_encoding])](#hashupdatedata-inputencoding)
* [hash.digest([encoding])](#hashdigestencoding)

--------------------------------------------------

`Hash` 类是用于创建数据散列摘要的工具类。它可以以两种方式之一使用：

* 作为可读和可写的[流](../stream/)，写入数据并在可读端产生计算后的散列摘要。

* 使用 [hash.update()](#hashupdatedata-inputencoding) 和 [hash.digest()](#hashdigestencoding) 产生计算后的散列。

[crypto.createHash()](./crypto.md#cryptocreatehashalgorithm) 方法用于创建 `Hash` 实例。`Hash` 对象无法直接使用 `new` 关键词创建。

示例：将 `Hash` 对象用作流：

``` javascript
const crypto = require('crypto');
const hash = crypto.createHash('sha256');

hash.on('readable', () => {
    var data = hash.read();
    if (data)
        console.log(data.toString('hex'));
    // Prints:
    //   6a2da20943931e9834fc12cfe5bb47bbd9ae43489a30726962b576f4e3993e50
});

hash.write('some data to hash');
hash.end();
```

示例：使用 `Hash` 并导流：

``` javascript
const crypto = require('crypto');
const fs = require('fs');
const hash = crypto.createHash('sha256');

const input = fs.createReadStream('test.js');
input.pipe(hash).pipe(process.stdout);
```

示例：使用 [hash.update()](#hashupdatedata-inputencoding) 和 [hash.digest()](#hashdigestencoding) 方法：

``` javascript
const crypto = require('crypto');
const hash = crypto.createHash('sha256');

hash.update('some data to hash');
console.log(hash.digest('hex'));
// Prints:
//   6a2da20943931e9834fc12cfe5bb47bbd9ae43489a30726962b576f4e3993e50
```


## hash.update(data[, input_encoding])

使用给定的 `data` 更新哈希内容，给出的 `input_encoding` 编码，可以是 `'utf8'`、`'ascii'` 或 `'binary'`。如果没有提供 `encoding`，同时 `data` 是一个字符串，将强制使用 `'binary'` 编码。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

当它作为流时，可以在新数据上多次调用。


## hash.digest([encoding])

计算所有传入的数据的散列摘要（使用 [hash.update()](#hashupdatedata-inputencoding) 方法）。`encoding` 可以是 `'hex'`、`'binary'` 或 `'base64'`。如果提供了 `encoding`，会返回一个字符串；否则返回一个 [Buffer](../buffer/)。

在调用 `hash.digest()` 方法之后，`Hash` 对象将不能再次使用。多次调用将导致抛出错误。