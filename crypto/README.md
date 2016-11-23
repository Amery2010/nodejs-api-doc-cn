# 加密(Crypto)

> 稳定度：2 - 稳定

该 `crypto` 模块提供了加密功能，包括一组用于包装 OpenSSL 的哈希，HMAC，加密，解密，签名和验证函数。

使用 `require('crypto')` 来访问这个模块。

``` javascript
const crypto = require('crypto');

const secret = 'abcdefg';
const hash = crypto.createHmac('sha256', secret)
                .update('I love cupcakes')
                .digest('hex');
console.log(hash);
// Prints:
//   c0fa1bc00531bd78ef38c628449c5102aeabd49b5dc3a2a516ea6ea959d6658e
```