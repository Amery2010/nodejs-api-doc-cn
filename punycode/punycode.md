# 方法和属性

#### 属性

* [punycode.version](#version)
* [punycode.ucs2](#ucs2)
  - [punycode.ucs2.encode(codePoints)](#ucs2_encode)
  - [punycode.ucs2.decode(string)](#ucs2_decode)

#### 方法

* [punycode.encode(string)](#encode)
* [punycode.decode(string)](#decode)
* [punycode.toUnicode(domain)](#toUnicode)
* [punycode.toASCII(domain)](#toASCII)

--------------------------------------------------


<div id="version" class="anchor"></div>
## punycode.version

一个代表当前 Punycode.js 版本号的字符串。


<div id="ucs2" class="anchor"></div>
## punycode.ucs2


<div id="ucs2_encode" class="anchor"></div>
#### punycode.ucs2.encode(codePoints)

创建一个包含字符串中每个 Unicode 符号的数字编码点的数组。由于 [JavaScript 在内部使用 UCS-2](https://mathiasbynens.be/notes/javascript-encoding)， 该函数会匹配 UTF-16 将一对替代半部（UCS-2 暴露的单独的字符）转换为单个编码点。

```javascript
punycode.ucs2.decode('abc'); // [0x61, 0x62, 0x63]
// surrogate pair for U+1D306 tetragram for centre:
punycode.ucs2.decode('\uD834\uDF06'); // [0x1D306]
```


<div id="ucs2_decode" class="anchor"></div>
#### punycode.ucs2.decode(string)

根据一组数字编码点值创建一个字符串。

```javascript
punycode.ucs2.encode([0x61, 0x62, 0x63]); // 'abc'
punycode.ucs2.encode([0x1D306]); // '\uD834\uDF06'
```


<div id="encode" class="anchor"></div>
## punycode.encode(string)

将一个 Unicode 符号的字符串转换为纯 ASCII 符号的 Punycode 字符串。

```javascript
// encode domain name parts
punycode.encode('mañana'); // 'maana-pta'
punycode.encode('☃-⌘'); // '--dqo34k'
```


<div id="decode" class="anchor"></div>
## punycode.decode(string)

将一个纯 ASCII 符号的 Punycode 字符串转换为 Unicode 符号的字符串。

```javascript
// decode domain name parts
punycode.decode('maana-pta'); // 'mañana'
punycode.decode('--dqo34k'); // '☃-⌘'
```


<div id="toUnicode" class="anchor"></div>
## punycode.toUnicode(domain)

将一个表示域名的 Punycode 字符串转换为 Unicode。只有域名中的 Punycode 部分会转换，也就是说你在一个已经转换为 Unicode 的字符串上调用它也是没问题的。

```javascript
// decode domain names
punycode.toUnicode('xn--maana-pta.com'); // 'mañana.com'
punycode.toUnicode('xn----dqo34k.com'); // '☃-⌘.com'
```


<div id="toASCII" class="anchor"></div>
## punycode.toASCII(domain)

将一个表示域名的 Unicode 字符串转换为 Punycode。只有域名的非 ASCII 部分会被转换，也就是说你在一个已经是 ASCII 的域名上调用它也是没问题的。

```javascript
// encode domain names
punycode.toASCII('mañana.com'); // 'xn--maana-pta.com'
punycode.toASCII('☃-⌘.com'); // 'xn----dqo34k.com'
```