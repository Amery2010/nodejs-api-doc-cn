# Certificate类

* [new crypto.Certificate()](#new-cryptocertificate)
* [certificate.exportPublicKey(spkac)](#certificateexportpublickeyspkac)
* [certificate.exportChallenge(spkac)](#certificateexportchallengespkac)
* [certificate.verifySpkac(spkac)](#certificateverifyspkacspkac)

--------------------------------------------------

SPKAC 是 Netscape 最初实现的证书签名请求机制并且现在正式指定为 [HTML5 的 keygen 元素](http://www.w3.org/TR/html5/forms.html#the-keygen-element)的一部分。

`crypto` 模块提供了 `Certificate` 类用于处理 SPKAC 数据。最常见的用法是处理 HTML5 `<keygen>` 元素生成的输出。Node.js 内部使用 OpenSSL 的 SPKAC 来实现。


## new crypto.Certificate()

`Certificate` 类的实例可以通过使用 `new` 关键词或调用 `crypto.Certificate()` 函数来创建：

``` javascript
const crypto = require('crypto');

const cert1 = new crypto.Certificate();
const cert2 = crypto.Certificate();
```


## certificate.exportPublicKey(spkac)

`spkac` 数据结构包括公钥和咨询。`certificate.exportPublicKey()` 以 Node.js 的 [Buffer](../buffer/) 形式返回公钥组件。`spkac` 参数既可以是字符串也可以是 [Buffer](../buffer/)。

``` javascript
const cert = require('crypto').Certificate();
const spkac = getSpkacSomehow();
const publicKey = cert.exportPublicKey(spkac);
console.log(publicKey);
// Prints the public key as <Buffer ...>
```


## certificate.exportChallenge(spkac)

`spkac` 数据结构包括公钥和咨询。`certificate.exportChallenge()` 以 Node.js 的 [Buffer](../buffer/) 形式返回公钥组件。`spkac` 参数既可以是字符串也可以是 [Buffer](../buffer/)。

``` javascript
const cert = require('crypto').Certificate();
const spkac = getSpkacSomehow();
const challenge = cert.exportChallenge(spkac);
console.log(challenge.toString('utf8'));
// Prints the challenge as a UTF8 string
```


## certificate.verifySpkac(spkac)

如果给定的 `spkac` 数据结构有效，则返回 `true`，否则，返回 `false`。`spkac` 参数必须是  Node.js 的 [Buffer](../buffer/) 形式。

``` javascript
const cert = require('crypto').Certificate();
const spkac = getSpkacSomehow();
console.log(cert.verifySpkac(new Buffer(spkac)));
// Prints true or false
```