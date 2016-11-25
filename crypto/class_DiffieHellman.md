# DiffieHellman类

* [diffieHellman.verifyError](#diffiehellmanverifyerror)
* [diffieHellman.generateKeys([encoding])](#diffiehellmangeneratekeysencoding)
* [diffieHellman.getGenerator([encoding])](#diffiehellmangetgeneratorencoding)
* [diffieHellman.setPublicKey(public_key[, encoding])](#diffiehellmansetpublickeypublickey-encoding)
* [diffieHellman.getPublicKey([encoding])](#diffiehellmangetpublickeyencoding)
* [diffieHellman.setPrivateKey(private_key[, encoding])](#diffiehellmansetprivatekeyprivatekey-encoding)
* [diffieHellman.getPrivateKey([encoding])](#diffiehellmangetprivatekeyencoding)
* [diffieHellman.getPrime([encoding])](#diffiehellmangetprimeencoding)
* [diffieHellman.computeSecret(other_public_key[, input_encoding][, output_encoding])](#diffiehellmancomputesecretotherpublickey-inputencoding-outputencoding)

--------------------------------------------------

`DiffieHellman` 类是一个用于创建 Diffie-Hellman 密钥交换的工具类。

`DiffieHellman` 类的实例可以使用 [crypto.createDiffieHellman()](./crypto.md#cryptocreatediffiehellmanprime-primeencoding-generator-generatorencoding) 函数来创建。

``` javascript
const crypto = require('crypto');
const assert = require('assert');

// Generate Alice's keys...
const alice = crypto.createDiffieHellman(2048);
const alice_key = alice.generateKeys();

// Generate Bob's keys...
const bob = crypto.createDiffieHellman(alice.getPrime(), alice.getGenerator());
const bob_key = bob.generateKeys();

// Exchange and generate the secret...
const alice_secret = alice.computeSecret(bob_key);
const bob_secret = bob.computeSecret(alice_key);

// OK
assert.equal(alice_secret.toString('hex'), bob_secret.toString('hex'));
```


## diffieHellman.verifyError

一个包含检查在执行 `DiffieHellman` 对象初始化过程中所产生的任何警告和/或错误的位字段。

以下值对此属性有效（定义在 `constants` 模块中）：

* `DH_CHECK_P_NOT_SAFE_PRIME`

* `DH_CHECK_P_NOT_PRIME`

* `DH_UNABLE_TO_CHECK_GENERATOR`

* `DH_NOT_SUITABLE_GENERATOR`


## diffieHellman.generateKeys([encoding])

生成私有和公开 Diffie-Hellman 键值，并返回指定 `encoding` 的公钥。这个密钥应该转移到另一方。编码可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding` 则返回一个字符串，否则返回一个 [Buffer](../buffer/)。


## diffieHellman.getGenerator([encoding])

返回指定 `encoding` 的 Diffie-Hellman 生成器。`encoding` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding` 则返回一个字符串，否则返回一个 [Buffer](../buffer/)。


## diffieHellman.setPublicKey(public_key[, encoding])

设置一个 Diffie-Hellman 公钥。如果提供了 `encoding` 参数，且是 `'binary'`、`'hex'` 或 `'base64'` 中的一种，则 `public_key` 期望是一个字符串。如果没有提供 `encoding`，则 `public_key` 期望是一个 [Buffer](../buffer/)。


## diffieHellman.getPublicKey([encoding])

返回指定 `encoding` 的 Diffie-Hellman 公钥。`encoding` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding` 则返回一个字符串，否则返回一个 [Buffer](../buffer/)。


## diffieHellman.setPrivateKey(private_key[, encoding])

设置一个 Diffie-Hellman 私钥。如果提供了 `encoding` 参数，且是 `'binary'`、`'hex'` 或 `'base64'` 中的一种，则 `private_key` 期望是一个字符串。如果没有提供 `encoding`，则 `private_key` 期望是一个 [Buffer](../buffer/)。


## diffieHellman.getPrivateKey([encoding])

返回指定 `encoding` 的 Diffie-Hellman 私钥。`encoding` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding` 则返回一个字符串，否则返回一个 [Buffer](../buffer/)。


## diffieHellman.getPrime([encoding])

返回指定 `encoding` 的 Diffie-Hellman 素数。`encoding` 可以是 `'binary'`、`'hex'` 或 `'base64'`。如果提供了 `encoding` 则返回一个字符串，否则返回一个 [Buffer](../buffer/)。


## diffieHellman.computeSecret(other_public_key[, input_encoding][, output_encoding])

使用 `other_public_key` 作为对方的公钥计算共享密钥并返回计算后的共享密钥。使用指定的 `input_encoding` 解释提供的密钥，并返回使用指定 `output_encoding` 编码的密钥。编码可以是 `'binary'`、`'hex'` 或 `'base64'`。如果没有提供 `input_encoding`，`other_public_key` 期望是一个 [Buffer](../buffer/)。

如果给出了 `output_encoding`，将返回一个字符串；否则，返回一个 [Buffer](../buffer/)。