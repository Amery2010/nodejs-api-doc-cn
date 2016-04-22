# 字符串解码(String Decoder)

> 稳定度：2 - 稳定

通过 `require('string_decoder')` 使用该模块。这个模块将一个 buffer 解码成一个字符串。它是 `buffer.toString()` 的一个简单接口，但提供对 utf8 的支持。

```javascript
const StringDecoder = require('string_decoder').StringDecoder;
const decoder = new StringDecoder('utf8');

const cent = new Buffer([0xC2, 0xA2]);
console.log(decoder.write(cent));

const euro = new Buffer([0xE2, 0x82, 0xAC]);
console.log(decoder.write(euro));
```