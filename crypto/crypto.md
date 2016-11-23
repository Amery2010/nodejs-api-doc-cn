# 方法和属性

* [crypto.DEFAULT_ENCODING](#cryptodefaultencoding)
* [crypto.createCipher(algorithm, password)](#cryptocreatecipheralgorithm-password)
* [crypto.createCipheriv(algorithm, key, iv)](#cryptocreatecipherivalgorithm-key-iv)
* [crypto.createCredentials(details)](#cryptocreatecredentialsdetails)
* [crypto.createDecipher(algorithm, password)](#cryptocreatedecipheralgorithm-password)
* [crypto.createDecipheriv(algorithm, key, iv)](#cryptocreatedecipherivalgorithm-key-iv)
* [crypto.createDiffieHellman(prime[, prime_encoding][, generator][, generator_encoding])](#cryptocreatediffiehellmanprime-primeencoding-generator-generatorencoding)
* [crypto.createDiffieHellman(prime_length[, generator])](#cryptocreatediffiehellmanprimelength-generator)
* [crypto.createECDH(curve_name)](#cryptocreateecdhcurvename)
* [crypto.createHash(algorithm)](#cryptocreatehashalgorithm)
* [crypto.createHmac(algorithm, key)](#cryptocreatehmacalgorithm-key)
* [crypto.createSign(algorithm)](#cryptocreatesignalgorithm)
* [crypto.createVerify(algorithm)](#cryptocreateverifyalgorithm)
* [crypto.getCiphers()](#cryptogetciphers)
* [crypto.getCurves()](#cryptogetcurves)
* [crypto.getDiffieHellman(group_name)](#cryptogetdiffiehellmangroupname)
* [crypto.getHashes()](#cryptogethashes)
* [crypto.pbkdf2(password, salt, iterations, keylen[, digest], callback)](#cryptopbkdf2password-salt-iterations-keylen-digest-callback)
* [crypto.pbkdf2Sync(password, salt, iterations, keylen[, digest])](#cryptopbkdf2syncpassword-salt-iterations-keylen-digest)
* [crypto.privateEncrypt(private_key, buffer)](#cryptoprivateencryptprivatekey-buffer)
* [crypto.privateDecrypt(private_key, buffer)](#cryptoprivatedecryptprivatekey-buffer)
* [crypto.publicEncrypt(public_key, buffer)](#cryptopublicencryptpublickey-buffer)
* [crypto.publicDecrypt(public_key, buffer)](#cryptopublicdecryptpublickey-buffer)
* [crypto.randomBytes(size[, callback])](#cryptorandombytessize-callback)
* [crypto.setEngine(engine[, flags])](#cryptosetengineengine-flags)

--------------------------------------------------


## crypto.DEFAULT_ENCODING

用于可以接受字符串或 [buffers](../buffer/) 的函数的默认编码。默认值为 `'buffer'`，这也使得这些方法的默认值为 [Buffer](../buffer/) 对象。

`crypto.DEFAULT_ENCODING` 机制用于处理期望值为 `'binary'` 的旧程序的向后兼容性。

新的应用程序应该期望默认值为 `'buffer'`。此属性在将来的 Node.js 版本中可能会被弃用。


## crypto.createCipher(algorithm, password)

使用给定的 `algorithm` 和 `password` 创建并返回一个 `Cipher` 对象。

`algorithm` 依赖于 OpenSSL，例如 `'aes192'` 等。在最新的 OpenSSL 版本中，`openssl list-cipher-algorithms` 会显示可用的加密算法。

`password` 用于导出密钥和初始化向量（IV）。该值必须是 `'binary'` 编码的字符串或 [Buffer](../buffer/)。

`crypto.createCipher()` 的实现是使用 OpenSSL 的函数 [EVP_BytesToKey](https://www.openssl.org/docs/crypto/EVP_BytesToKey.html) 导出密钥，摘要算法设置为 MD5，一次迭代，没有盐。缺少盐将允许字典攻击，因为相同的密码总是创建相同的密钥。低迭代计数和非加密安全散列算法使得密码测试非常快。

OpenSSL 一致建议使用 pbkdf2 代替 [EVP_BytesToKey](https://www.openssl.org/docs/crypto/EVP_BytesToKey.html)，建议开发人员自己使用 [crypto.pbkdf2()](#cryptopbkdf2password-salt-iterations-keylen-digest-callback) 导出密钥和 IV，并使用 [crypto.createCipheriv()](#cryptocreatecipherivalgorithm-key-iv) 创建 `Cipher` 对象。


## crypto.createCipheriv(algorithm, key, iv)

使用给定的 `algorithm`、`password` 和初始化向量（`iv`）创建并返回一个 `Cipher` 对象。

`algorithm` 依赖于 OpenSSL，例如 `'aes192'` 等。在最新的 OpenSSL 版本中，`openssl list-cipher-algorithms` 会显示可用的加密算法。

`key` 是 `algorithm` 使用的原始密钥，同时 `iv` 是一个 [初始化向量](https://en.wikipedia.org/wiki/Initialization_vector)。所有参数都必须是 `'binary'` 编码的字符串或 [buffers](../buffer/)。


## crypto.createCredentials(details)

> 稳定度：0 - 已废弃：使用 [tls.createSecureContext()](../tls/tls.md#tlscreatesecurecontextoptions) 代替。

`crypto.createCredentials()` 方法是用于创建的已弃用别名并返回一个 `tls.SecureContext` 对象。不应该去使用 `crypto.createCredentials()` 方法。

可选的 `details` 参数是一个带有键值的哈希对象：

* `pfx`：{String} | {Buffer} - PFX 或 PKCS12 编码的私钥、证书和 CA 证书。

* `key`：{String} - PEM 编码的私钥。

* `passphrase`：{String} - 私钥或 PFX 的密码。

* `cert`：{String} - PEM 编码的证书。

* `ca`：{String} | {Array} - PEM 编码的可信任 CA 证书的字符串或字符串数组。

* `crl`：{String} | {Array} - PEM 编码的 CRL（证书吊销列表）的字符串或字符串数组。

* `ciphers`：{String} - 使用 [OpenSSL 加密列表格式](https://www.openssl.org/docs/apps/ciphers.html#CIPHER_LIST_FORMAT)来描述要使用或排除的加密算法。

如果没有给出 `'ca'` 的详细信息，Node.js 会使用 Mozilla 的默认[公共信任 CA 列表](https://mxr.mozilla.org/mozilla/source/security/nss/lib/ckfw/builtins/certdata.txt)。


## crypto.createDecipher(algorithm, password)

使用给定的 `algorithm` 和 `password`（密钥）创建并返回一个 `Decipher` 对象。

`crypto.createDecipher()` 的实现是使用 OpenSSL 的函数 [EVP_BytesToKey](https://www.openssl.org/docs/crypto/EVP_BytesToKey.html) 导出密钥，摘要算法设置为 MD5，一次迭代，没有盐。缺少盐将允许字典攻击，因为相同的密码总是创建相同的密钥。低迭代计数和非加密安全散列算法使得密码测试非常快。

OpenSSL 一致建议使用 pbkdf2 代替 [EVP_BytesToKey](https://www.openssl.org/docs/crypto/EVP_BytesToKey.html)，建议开发人员自己使用 [crypto.pbkdf2()](#cryptopbkdf2password-salt-iterations-keylen-digest-callback) 导出密钥和 IV，并使用 [crypto.createCipheriv()](#cryptocreatecipherivalgorithm-key-iv) 创建 `Decipher` 对象。


## crypto.createDecipheriv(algorithm, key, iv)

使用给定的 `algorithm`、`password` 和初始化向量（`iv`）创建并返回一个 `Decipher` 对象。

`algorithm` 依赖于 OpenSSL，例如 `'aes192'` 等。在最新的 OpenSSL 版本中，`openssl list-cipher-algorithms` 会显示可用的加密算法。

`key` 是 `algorithm` 使用的原始密钥，同时 `iv` 是一个 [初始化向量](https://en.wikipedia.org/wiki/Initialization_vector)。所有参数都必须是 `'binary'` 编码的字符串或 [buffers](../buffer/)。


## crypto.createDiffieHellman(prime[, prime_encoding][, generator][, generator_encoding])

使用提供的 `prime` 和可选的特定 `generator` 创建一个 `DiffieHellman` 密钥交换对象。

`generator` 参数应该是一个数字、字符串或 [Buffer](../buffer/)。如果没有指定 `generator`，将使用值 `2`。

`prime_encoding` 和 `generator_encoding` 参数可以是 `'binary'`、`'hex'` 或 `'base64'`。

如果指定了 `prime_encoding`，`prime` 期望是一个字符串或 [Buffer](../buffer/)。

如果指定了 `generator_encoding`，`generator` 期望是一个字符串、数字或 [Buffer](../buffer/)。


## crypto.createDiffieHellman(prime_length[, generator])

创建并使用可选的特定数字 `generator` 生成一个主要的 `prime_length` 位 `DiffieHellman` 密钥交换对象。如果没有指定 `generator`，将使用值 `2`。


## crypto.createECDH(curve_name)

使用由 `curve_name` 字符串指定的预定义曲线创建一个 Elliptic Curve Diffie-Hellman（`ECDH`）密钥交换对象。使用 [crypto.getCurves()](#cryptogetcurves) 来获取可用的曲线名称列表。在最新的 OpenSSL 版本中，`openssl ecparam -list_curves` 也会显示每个可用的椭圆曲线名称和描述。


## crypto.createHash(algorithm)

使用给定的 `algorithm` 创建并返回一个可以用于生成散列摘要的 `Hash` 对象。

`algorithm` 取决于平台上 OpenSSL 版本支持的可用算法。例如 `'sha256'`、`'sha512'` 等。在最新的 OpenSSL 版本中，`openssl list-message-digest-algorithms` 会显示可用的摘要算法。

示例：生成 sha256 的文件摘要：

``` javascript
const filename = process.argv[2];
const crypto = require('crypto');
const fs = require('fs');

const hash = crypto.createHash('sha256');

const input = fs.createReadStream(filename);
input.on('readable', () => {
    var data = input.read();
    if (data)
        hash.update(data);
    else {
        console.log(`${hash.digest('hex')} ${filename}`);
    }
});
```


## crypto.createHmac(algorithm, key)

使用给定的 `algorithm` 和 `key` 创建并返回一个 `Hmac` 对象。

`algorithm` 取决于平台上 OpenSSL 版本支持的可用算法。例如 `'sha256'`、`'sha512'` 等。在最新的 OpenSSL 版本中，`openssl list-message-digest-algorithms` 会显示可用的摘要算法。

该 `key` 是用于生成加密 HMAC 哈希的 HMAC 密钥。

示例：生成 sha256 的文件 HMAC：

``` javascript
const filename = process.argv[2];
const crypto = require('crypto');
const fs = require('fs');

const hmac = crypto.createHmac('sha256', 'a secret');

const input = fs.createReadStream(filename);
input.on('readable', () => {
    var data = input.read();
    if (data)
        hmac.update(data);
    else {
        console.log(`${hmac.digest('hex')} ${filename}`);
    }
});
```


## crypto.createSign(algorithm)

使用给定的 `algorithm` 创建并返回一个 `Sign` 对象。在最新的 OpenSSL 版本中，`openssl list-public-key-algorithms` 会显示可用的签名算法。例如 `'RSA-SHA256'`。


## crypto.createVerify(algorithm)

使用给定的 `algorithm` 创建并返回一个 `Verify` 对象。在最新的 OpenSSL 版本中，`openssl list-public-key-algorithms` 会显示可用的签名算法。例如 `'RSA-SHA256'`。


## crypto.getCiphers()

返回包含受支持的加密算法名称的数组。

示例：

``` javascript
const ciphers = crypto.getCiphers();
console.log(ciphers); // ['aes-128-cbc', 'aes-128-ccm', ...]
```


## crypto.getCurves()

返回包含受支持的椭圆曲线名称的数组。

示例：

``` javascript
const curves = crypto.getCurves();
console.log(curves); // ['secp256k1', 'secp384r1', ...]
```


## crypto.getDiffieHellman(group_name)

创建一个预定义的 `DiffieHellman` 密钥交换对象。支持的组是：`'modp1'`、`'modp2'`、`'modp5'`（在 [RFC 2412](https://www.rfc-editor.org/rfc/rfc2412.txt) 中定义，但请参阅[警告](./notes.md#support-for-weak-or-compromised-algorithms)）和 `'modp14'`、`'modp15'`、`'modp16'`、`'modp17'`、`'modp18'`（在 [RFC 3526](https://www.rfc-editor.org/rfc/rfc3526.txt) 中定义）。返回的对象模仿由 [crypto.createDiffieHellman()](#cryptocreatediffiehellmanprime-primeencoding-generator-generatorencoding) 创建的对象接口，但不允许更改密钥（例如用 [diffieHellman.setPublicKey()](./class_DiffieHellman.md#diffiehellmansetpublicKeypublickey-encoding)）。使用这种方法的优点是，各方不必预先生成或交换组模量，节省了处理器和通信的时间。

示例（获取共享密钥）：

``` javascript
const crypto = require('crypto');
const alice = crypto.getDiffieHellman('modp14');
const bob = crypto.getDiffieHellman('modp14');

alice.generateKeys();
bob.generateKeys();

const alice_secret = alice.computeSecret(bob.getPublicKey(), null, 'hex');
const bob_secret = bob.computeSecret(alice.getPublicKey(), null, 'hex');

/* alice_secret and bob_secret should be the same */
console.log(alice_secret == bob_secret);
```


## crypto.getHashes()

返回包含受支持的散列算法名称的数组。

示例：

``` javascript
const hashes = crypto.getHashes();
console.log(hashes); // ['sha', 'sha1', 'sha1WithRSAEncryption', ...]
```


## crypto.pbkdf2(password, salt, iterations, keylen[, digest], callback)

提供了一个异步的 Password-Based Key Derivation Function 2（PBKDF2）的实现。通过 `digest` 指定所选的 HMAC 摘要算法从 `password`、`salt` 和 `iterations` 应用于导出所请求的字节长度（`keylen`）的密钥。如果没有指定 `digest` 算法，将使用默认的 `'sha1'`。

提供的 `callback` 函数在调用时带有两个参数：`err` 和 `derivedKey`。如果发生错误，`err` 将被设置；否则，`err` 会是 `null`。成功生成的 `derivedKey` 将作为 [Buffer](../buffer/) 传递。

`iterations` 参数必须设置为尽可能大的数字。迭代次数越高，导出密钥将更安全，但需要更长的时间来完成。

`salt` 应该尽可能地唯一。建议盐是随机的，它们的长度应该大于 16 字节。详见 [NIST SP 800-132](http://csrc.nist.gov/publications/nistpubs/800-132/nist-sp800-132.pdf)。

示例：

``` javascript
const crypto = require('crypto');
crypto.pbkdf2('secret', 'salt', 100000, 512, 'sha512', (err, key) => {
    if (err) throw err;
    console.log(key.toString('hex')); // 'c5e478d...1469e50'
});
```

可以使用 [crypto.getHashes()](#cryptogethashes) 检索支持的摘要函数数组。


## crypto.pbkdf2Sync(password, salt, iterations, keylen[, digest])

提供了一个同步的 Password-Based Key Derivation Function 2（PBKDF2）的实现。通过 `digest` 指定所选的 HMAC 摘要算法从 `password`、`salt` 和 `iterations` 应用于导出所请求的字节长度（`keylen`）的密钥。如果没有指定 `digest` 算法，将使用默认的 `'sha1'`。

提供的 `callback` 函数在调用时带有两个参数：`err` 和 `derivedKey`。如果发生错误，`err` 将被设置；否则，`err` 会是 `null`。成功生成的 `derivedKey` 将作为 [Buffer](../buffer/) 传递。

`iterations` 参数必须设置为尽可能大的数字。迭代次数越高，导出密钥将更安全，但需要更长的时间来完成。

`salt` 应该尽可能地唯一。建议盐是随机的，它们的长度应该大于 16 字节。详见 [NIST SP 800-132](http://csrc.nist.gov/publications/nistpubs/800-132/nist-sp800-132.pdf)。

示例：

``` javascript
const crypto = require('crypto');
const key = crypto.pbkdf2Sync('secret', 'salt', 100000, 512, 'sha512');
console.log(key.toString('hex'));  // 'c5e478d...1469e50'
```

可以使用 [crypto.getHashes()](#cryptogethashes) 检索支持的摘要函数数组。


## crypto.privateEncrypt(private_key, buffer)

使用 `private_key` 加密 `buffer`。

`private_key` 可以是一个对象或字符串。如果 `private_key` 是一个字符串，它被视为没有密码短语密钥并使用 `RSA_PKCS1_OAEP_PADDING`。如果 `private_key` 是一个对象，它被解释为带有键值的哈希对象：

* `key`：{String} - PEM 编码的私钥。

* `passphrase`：{String} - 私钥的可选密码。

* `padding`：可选的填充值，是以下值之一：

    - `constants.RSA_NO_PADDING`
    
    - `constants.RSA_PKCS1_PADDING`
    
    - `constants.RSA_PKCS1_OAEP_PADDING`
    
所有的填充值都定义在 `constants` 模块中。


## crypto.privateDecrypt(private_key, buffer)

使用 `private_key` 解密 `buffer`。

`private_key` 可以是一个对象或字符串。如果 `private_key` 是一个字符串，它被视为没有密码短语密钥并使用 `RSA_PKCS1_OAEP_PADDING`。如果 `private_key` 是一个对象，它被解释为带有键值的哈希对象：

* `key`：{String} - PEM 编码的私钥。

* `passphrase`：{String} - 私钥的可选密码。

* `padding`：可选的填充值，是以下值之一：

    - `constants.RSA_NO_PADDING`
    
    - `constants.RSA_PKCS1_PADDING`
    
    - `constants.RSA_PKCS1_OAEP_PADDING`
    
所有的填充值都定义在 `constants` 模块中。


## crypto.publicEncrypt(public_key, buffer)

使用 `public_key` 加密 `buffer`。

`public_key` 可以是一个对象或字符串。如果 `public_key` 是一个字符串，它被视为没有密码短语密钥并使用 `RSA_PKCS1_OAEP_PADDING`。如果 `public_key` 是一个对象，它被解释为带有键值的哈希对象：

* `key`：{String} - PEM 编码的公钥。

* `passphrase`：{String} - 私钥的可选密码。

* `padding`：可选的填充值，是以下值之一：

    - `constants.RSA_NO_PADDING`
    
    - `constants.RSA_PKCS1_PADDING`
    
    - `constants.RSA_PKCS1_OAEP_PADDING`

因为 RSA 公钥可以从私钥导出，所以可以传递私钥而不是公钥。

所有的填充值都定义在 `constants` 模块中。


## crypto.publicDecrypt(public_key, buffer)

使用 `public_key` 解密 `buffer`。

`public_key` 可以是一个对象或字符串。如果 `public_key` 是一个字符串，它被视为没有密码短语密钥并使用 `RSA_PKCS1_OAEP_PADDING`。如果 `public_key` 是一个对象，它被解释为带有键值的哈希对象：

* `key`：{String} - PEM 编码的公钥。

* `passphrase`：{String} - 私钥的可选密码。

* `padding`：可选的填充值，是以下值之一：

    - `constants.RSA_NO_PADDING`
    
    - `constants.RSA_PKCS1_PADDING`
    
    - `constants.RSA_PKCS1_OAEP_PADDING`

因为 RSA 公钥可以从私钥导出，所以可以传递私钥而不是公钥。

所有的填充值都定义在 `constants` 模块中。


## crypto.randomBytes(size[, callback])

以加密方式生成强的伪随机数据。`size` 参数是指示要生成的字节数的数字。

如果提供了 `callback` 函数，将异步生成字节并且 `callback` 函数的调用会带有两个参数：`err` 和 `buf`。如果发生错误，`err` 会是一个 [Error](../errors/) 对象；否则会是 `null`。`buf` 参数会是一个包含生成的字节的 [Buffer](../buffer/)。

``` javascript
// 异步
const crypto = require('crypto');
crypto.randomBytes(256, (err, buf) => {
    if (err) throw err;
    console.log(`${buf.length} bytes of random data: ${buf.toString('hex')}`);
});
```

如果没有提供 `callback` 函数，将同步产生随机字节并作为 [Buffer](../buffer/) 返回。如果生成字节时出现问题，将抛出错误。

``` javascript
// 同步
const buf = crypto.randomBytes(256);
console.log(`${buf.length} bytes of random data: ${buf.toString('hex')}`);
```

`crypto.randomBytes()` 方法会造成阻塞直到有足够的熵。这通常不会花费多于几毫秒的时间。在启动后，当整个系统仍然处于低熵时，可想而知，在产生随机字节时，它将会阻塞更长的时间。


## crypto.setEngine(engine[, flags])

加载和设置某些或所有的 OpenSSL 函数的 `engine`（取决于 `flags`）。

`engine` 可以是引擎共享库的 id 或路径。

可选的 `flags` 参数默认使用 `ENGINE_METHOD_ALL`。`flags` 是一个位字段，采用以下标志之一或其混合（定义在 `constants` 模块中）：

* `ENGINE_METHOD_RSA`

* `ENGINE_METHOD_DSA`

* `ENGINE_METHOD_DH`

* `ENGINE_METHOD_RAND`

* `ENGINE_METHOD_ECDH`

* `ENGINE_METHOD_ECDSA`

* `ENGINE_METHOD_CIPHERS`

* `ENGINE_METHOD_DIGESTS`

* `ENGINE_METHOD_STORE`

* `ENGINE_METHOD_PKEY_METHS`

* `ENGINE_METHOD_PKEY_ASN1_METHS`

* `ENGINE_METHOD_ALL`

* `ENGINE_METHOD_NONE`