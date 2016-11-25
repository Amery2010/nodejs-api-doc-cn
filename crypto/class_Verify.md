# Verify类

* [verifier.update(data[, input_encoding])](#verifierupdatedata-inputencoding)
* [verifier.verify(object, signature[, signature_format])](#verifierverifyobject-signature-signatureformat)

--------------------------------------------------

`Verify` 类是用于验证签名的工具类。它可以以两种方式之一使用：

* 作为一个可写[流](../stream/)，写入数据用于根据所提供的签名进行验证。

* 使用 [verify.update()](#verifierupdatedata-inputencoding) 和 [verify.verify()](#verifierverifyobject-signature-signatureformat) 方法来验证签名。

[crypto.createVerify()](./crypto.md#cryptocreateverifyalgorithm) 方法用于创建 `Verify` 实例。`Verify` 对象无法直接使用 `new` 关键词创建。

示例：将 `Verify` 对象用作流：

``` javascript
const crypto = require('crypto');
const verify = crypto.createVerify('RSA-SHA256');

verify.write('some data to sign');
verify.end();

const public_key = getPublicKeySomehow();
const signature = getSignatureToVerify();
console.log(sign.verify(public_key, signature));
// Prints true or false
```

示例：使用 [verify.update()](#verifierupdatedata-inputencoding) 和 [verify.verify()](#verifierverifyobject-signature-signatureformat) 方法：

``` javascript
const crypto = require('crypto');
const verify = crypto.createVerify('RSA-SHA256');

verify.update('some data to sign');

const public_key = getPublicKeySomehow();
const signature = getSignatureToVerify();
console.log(verify.verify(public_key, signature));
// Prints true or false
```


## verifier.update(data[, input_encoding])

用给定的 `data` 更新 `Verify` 内容，给出的 `input_encoding` 编码，可以是 `'utf8'`、`'ascii'` 或 `'binary'`。如果没有提供 `encoding`，同时 `data` 是一个字符串，将强制使用 `'utf8'` 编码。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

当它作为流时，可以在新数据上多次调用。


## verifier.verify(object, signature[, signature_format])

使用给定的 `object` 和 `signature` 验证提供的数据。`object` 参数是一个包含 PEM 编码对象的字符串，它可以是 RSA 公钥、DSA 公钥或一个 X.509 证书。`signature` 参数是先前计算的数据的签名，`signature_format` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果指定了 `signature_format`，`signature` 期望是一个字符串，否则期望是一个 [Buffer](../buffer/)。

返回 `true` 或 `false` 取决于数据和公钥的签名的有效性。

在调用 `verify.verify()` 方法之后，`Verify` 对象将不能再次使用。多次调用 `verify.verify()` 将导致抛出错误。