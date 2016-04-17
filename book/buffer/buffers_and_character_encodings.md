# Buffers和字符编码

Buffers 通常用于代表编码的字符序列，比如 UTF8 、 UCS2 、 Base64 甚至 Hex-encoded 的数据。有可能通过使用一个明确的编码方法在 Buffers 和普通的 JavaScript 字符串对象之间进行相互转换。

```javascript
const buf = Buffer.from('hello world', 'ascii');
console.log(buf.toString('hex'));
// prints: 68656c6c6f20776f726c64
console.log(buf.toString('base64'));
// prints: aGVsbG8gd29ybGQ=
```

Node.js 目前支持的字符编码包括：

* `'ascii'` - 仅支持 7位 ASCII 数据。如果设置去掉高位的话，这种编码方法是非常快的。

* `'utf8'` - 多字节编码的Unicode字符。许多网页和其他文档格式使用 UTF-8 。

* `'utf16le'` - 2或4个字节，小端编码的Unicode字符。支持代理对（U+10000 to U+10FFFF）。

* `'ucs2'` - `'utf16le'` 的别名。

* `'base64'` - Base64 字符串编码。当从一个字符串创建一个 buffer 时，按照 [RFC 4648, Section 5](https://tools.ietf.org/html/rfc4648#section-5) 里的规定，这种编码也将接受正确的“URL和文件名安全字母”。

* `'binary'` - 一种把 buffer 编码成一字节（latin-1）编码字符串的方式。目前不支持 `'latin-1'` 字符串。通过 `'binary'` 来代替 `'latin-1'` 使用 `'latin-1'` 编码。

* `'hex'` - 将每个字节编码为两个十六进制字符。

