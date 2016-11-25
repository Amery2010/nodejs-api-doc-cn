# Hmac类

* [hmac.update(data[, input_encoding])](#hmacupdatedata-inputencoding)
* [hmac.digest([encoding])](#hmacdigestencoding)

--------------------------------------------------

`Hmac` 类是用于创建加密的 HMAC 摘要的工具类。它可以以两种方式之一使用：

* 作为可读和可写的[流](../stream/)，写入数据并在可读端产生计算后的 HMAC 摘要。

* 使用 [hmac.update()](#hmacupdatedata-inputencoding) 和 [hmac.digest()](#hmacdigestencoding) 产生计算后的 HMAC 摘要。

[crypto.createHmac()](./crypto.md##cryptocreatehmacalgorithm-key) 方法用于创建 `Hmac` 实例。`Hmac` 对象无法直接使用 `new` 关键词创建。

示例：将 `Hmac` 对象用作流：

``` javascript
const crypto = require('crypto');
const hmac = crypto.createHmac('sha256', 'a secret');

hmac.on('readable', () => {
    var data = hmac.read();
    if (data)
        console.log(data.toString('hex'));
    // Prints:
    //   7fd04df92f636fd450bc841c9418e5825c17f33ad9c87c518115a45971f7f77e
});

hmac.write('some data to hash');
hmac.end();
```

示例：使用 `Hmac` 并导流：

``` javascript
const crypto = require('crypto');
const fs = require('fs');
const hmac = crypto.createHmac('sha256', 'a secret');

const input = fs.createReadStream('test.js');
input.pipe(hmac).pipe(process.stdout);
```

示例：使用 [hmac.update()](#hmacupdatedata-inputencoding) 和 [hmac.digest()](#hmacdigestencoding) 方法：

``` javascript
const crypto = require('crypto');
const hmac = crypto.createHmac('sha256', 'a secret');

hmac.update('some data to hash');
console.log(hmac.digest('hex'));
// Prints:
//   7fd04df92f636fd450bc841c9418e5825c17f33ad9c87c518115a45971f7f77e
```


## hmac.update(data[, input_encoding])

使用给定的 `data` 更新 HMAC 内容，给出的 `input_encoding` 编码，可以是 `'utf8'`、`'ascii'` 或 `'binary'`。如果没有提供 `encoding`，同时 `data` 是一个字符串，将强制使用 `'binary'` 编码。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

当它作为流时，可以在新数据上多次调用。


## hmac.digest([encoding])

计算所有传入的的数据的 HMAC 摘要（使用 [hmac.update()](#hmacupdatedata-inputencoding) 方法）。`encoding` 可以是 `'hex'`、`'binary'` 或 `'base64'`。如果提供了 `encoding`，会返回一个字符串；否则返回一个 [Buffer](../buffer/)。

在调用 `hmac.digest()` 方法之后，`Hmac` 对象将不能再次使用。多次调用将导致抛出错误。