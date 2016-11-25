# Sign类

* [sign.update(data[, input_encoding])](#signupdatedata-inputencoding)
* [sign.sign(private_key[, output_format])](#signsignprivatekey-outputformat)

--------------------------------------------------

`Sign` 类是用于生成签名的工具类。它可以以两种方式之一使用：

* 作为一个可写[流](../stream/)，写入要签名的数据，[sign.sign()](#signsignprivatekey-outputformat) 用于生成和返回签名。

* 使用 [sign.update()](#signupdatedata-inputencoding) 和 [sign.sign()](#signsignprivatekey-outputformat) 方法来产生签名。

[crypto.createSign()](./crypto.md#cryptocreatesignalgorithm) 方法用于创建 `Sign` 实例。`Sign` 对象无法直接使用 `new` 关键词创建。

示例：将 `Sign` 对象用作流：

``` javascript
const crypto = require('crypto');
const sign = crypto.createSign('RSA-SHA256');

sign.write('some data to sign');
sign.end();

const private_key = getPrivateKeySomehow();
console.log(sign.sign(private_key, 'hex'));
// Prints the calculated signature
```

示例：使用 [sign.update()](#signupdatedata-inputencoding) 和 [sign.sign()](#signsignprivatekey-outputformat) 方法：

``` javascript
const crypto = require('crypto');
const sign = crypto.createSign('RSA-SHA256');

sign.update('some data to sign');

const private_key = getPrivateKeySomehow();
console.log(sign.sign(private_key, 'hex'));
// Prints the calculated signature
```

`sign` 实例也可以通过传入摘要算法名称来创建，在这种情况下 OpenSSL 将从 PEM 格式的私钥类型推断完全的签名算法，包括没有直接暴露名称常量的算法，如，`'ecdsa-with-SHA256'`。

示例：使用 SHA256 的 ECDSA 签名：

``` javascript
const crypto = require('crypto');
const sign = crypto.createSign('sha256');

sign.update('some data to sign');

const private_key = '-----BEGIN EC PRIVATE KEY-----\n' +
        'MHcCAQEEIF+jnWY1D5kbVYDNvxxo/Y+ku2uJPDwS0r/VuPZQrjjVoAoGCCqGSM49\n' +
        'AwEHoUQDQgAEurOxfSxmqIRYzJVagdZfMMSjRNNhB8i3mXyIMq704m2m52FdfKZ2\n' +
        'pQhByd5eyj3lgZ7m7jbchtdgyOF8Io/1ng==\n' +
        '-----END EC PRIVATE KEY-----\n';

console.log(sign.sign(private_key).toString('hex'));
```


## sign.update(data[, input_encoding])

用给定的 `data` 更新 `Sign` 内容，给出的 `input_encoding` 编码，可以是 `'utf8'`、`'ascii'` 或 `'binary'`。如果没有提供 `encoding`，同时 `data` 是一个字符串，将强制使用 `'utf8'` 编码。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

当它作为流时，可以在新数据上多次调用。


## sign.sign(private_key[, output_format])

使用 [sign.update()](#signupdatedata-inputencoding) 或 [sign.write()](../stream/api_for_stream_consumers.md#write) 计算所有传递的数据的签名。

`private_key` 参数可以是一个对象或字符串。如果 `private_key` 是一个字符串，它被视为没有密码的原始密钥。如果 `private_key` 是一个对象，它被解释为包含两个属性的哈希：

* `key`：{String} - PEM 编码的私钥

* `passphrase`：{String} - 私钥的密码

`output_format` 可以指定为 `'binary'`、`'hex'` 或 `'base64'` 中的一种。如果提供了 `output_format` 则返回一个字符串；否则返回一个 [Buffer](../buffer/)。

在调用 `sign.sign()` 方法之后，`Sign` 对象将不能再次使用。多次调用将导致抛出错误。