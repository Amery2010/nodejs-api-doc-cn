# ECDH类

* [ecdh.generateKeys([encoding[, format]])](#ecdhgeneratekeysencoding-format)
* [ecdh.setPrivateKey(private_key[, encoding])](#ecdhsetprivatekeyprivatekey-encoding)
* [ecdh.getPrivateKey([encoding])](#ecdhgetprivatekeyencoding)
* [ecdh.setPublicKey(public_key[, encoding])](#ecdhsetpublickeypublickey-encoding)
* [ecdh.getPublicKey([encoding[, format]])](#ecdhgetpublickeyencoding-format)
* [ecdh.computeSecret(other_public_key[, input_encoding][, output_encoding])](#ecdhcomputesecretotherpublickey-inputencoding-outputencoding)

--------------------------------------------------

`ECDH` 类是用来创建 Elliptic Curve Diffie-Hellman（ECDH）密钥交换的工具类。

`ECDH` 类的实例可以使用 [crypto.createECDH()](./crypto.md#cryptocreateecdhcurvename) 函数来创建。

``` javascript
const crypto = require('crypto');
const assert = require('assert');

// Generate Alice's keys...
const alice = crypto.createECDH('secp521r1');
const alice_key = alice.generateKeys();

// Generate Bob's keys...
const bob = crypto.createECDH('secp521r1');
const bob_key = bob.generateKeys();

// Exchange and generate the secret...
const alice_secret = alice.computeSecret(bob_key);
const bob_secret = bob.computeSecret(alice_key);

assert(alice_secret, bob_secret);
// OK
```


## ecdh.generateKeys([encoding[, format]])

生成私有的和公共的 EC Diffie-Hellman 键值，并返回指定 `format` 和 `encoding` 的公钥。这个密钥应该转移到另一方。

`format` 参数指定点编码，可以是 `'compressed'`、`'uncompressed'` 或 `'hybrid'`。如果没有指定 `format`，这个点会以 `'uncompressed'` 格式返回。

`encoding` 参数可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding`，会返回一个字符串；否则返回一个 [Buffer](../buffer/)。


## ecdh.setPrivateKey(private_key[, encoding])

设置一个 EC Diffie-Hellman 私钥。`encoding` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding`，`private_key` 则期望是个字符串；否则，`private_key` 期望是一个 [Buffer](../buffer/)。当 `ECDH` 对象被创建时，如果 `private_key` 指定的曲线无效，将抛出错误。在设置私钥时，还在 ECDH 对象中生成和设置了相关联的公共点（密钥）。


## ecdh.getPrivateKey([encoding])

返回指定了 `encoding` 的 EC Diffie-Hellman 私钥，它可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding`，会返回一个字符串；否则返回一个 [Buffer](../buffer/)。


## ecdh.setPublicKey(public_key[, encoding])

> 稳定度：0 - 已废弃

设置一个 EC Diffie-Hellman 公钥。密钥的编码可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `public_key` 则期望是个字符串；否则，期望是一个 [Buffer](../buffer/)。

请注意，通常没有理由调用此方法，因为 `ECDH` 只需要私钥和对方的公钥来计算共享密钥。通常会调用 [ecdh.generateKeys()](#ecdhgeneratekeysencoding-format) 或 [ecdh.setPrivateKey()](#ecdhsetprivatekeyprivatekey-encoding)。[ecdh.setPrivateKey()](#ecdhsetprivatekeyprivatekey-encoding) 方法尝试生成与正被设置的私钥相关联的公共点/密钥。

示例（获取共享密钥）：

``` javascript
const crypto = require('crypto');
const alice = crypto.createECDH('secp256k1');
const bob = crypto.createECDH('secp256k1');

// Note: This is a shortcut way to specify one of Alice's previous private
// keys. It would be unwise to use such a predictable private key in a real
// application.
alice.setPrivateKey(
    crypto.createHash('sha256').update('alice', 'utf8').digest()
);

// Bob uses a newly generated cryptographically strong
// pseudorandom key pair bob.generateKeys();

const alice_secret = alice.computeSecret(bob.getPublicKey(), null, 'hex');
const bob_secret = bob.computeSecret(alice.getPublicKey(), null, 'hex');

// alice_secret and bob_secret should be the same shared secret value
console.log(alice_secret === bob_secret);
```


## ecdh.getPublicKey([encoding[, format]])

返回指定了 `encoding` 和 `format` 的 EC Diffie-Hellman 公钥。

`format` 参数指定点编码，可以是 `'compressed'`、`'uncompressed'` 或 `'hybrid'`。如果没有指定 `format`，这个点会以 `'uncompressed'` 格式返回。

`encoding` 参数可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding`，会返回一个字符串；否则返回一个 [Buffer](../buffer/)。


## ecdh.computeSecret(other_public_key[, input_encoding][, output_encoding])

使用作为对方的公钥的 `other_public_key` 计算共享密钥，并返回计算后的共享密钥。使用指定的 `input_encoding` 解释提供的密钥，并返回使用指定 `output_encoding` 编码的密钥。编码可以是 `'binary'`、`'hex'` 或 `'base64'`。如果没有提供 `input_encoding`，`other_public_key` 期望是一个 [Buffer](../buffer/)。

如果给出了 `output_encoding`，将返回一个字符串；否则，返回一个 [Buffer](../buffer/)。