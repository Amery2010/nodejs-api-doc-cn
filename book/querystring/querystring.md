# 方法和属性

* [querystring.stringify(obj[, sep][, eq][, options])](#stringify)
* [querystring.parse(str[, sep][, eq][, options])](#parse)
* [querystring.escape](#escape)
* [querystring.unescape](#unescape)

--------------------------------------------------


<div id="stringify" class="anchor"></div>
## querystring.stringify(obj[, sep][, eq][, options])

序列化一个对象到一个查询字符串。可以选择是否覆盖默认的分割符（`'&'`）和分配符（`'='`）。

Options 对象可能包含 `encodeURIComponent` 属性（默认为 `querystring.escape`）。在有必要时，它可以用 `non-utf8` 码来编码字符串。

例子：

```javascript
querystring.stringify({
        foo: 'bar',
        baz: ['qux', 'quux'],
        corge: ''
    })
    // returns 'foo=bar&baz=qux&baz=quux&corge='

querystring.stringify({
        foo: 'bar',
        baz: 'qux'
    }, ';', ':')
    // returns 'foo:bar;baz:qux'

// Suppose gbkEncodeURIComponent function already exists,
// it can encode string with `gbk` encoding
querystring.stringify({
        w: '中文',
        foo: 'bar'
    }, null, null, {
        encodeURIComponent: gbkEncodeURIComponent
    })
    // returns 'w=%D6%D0%CE%C4&foo=bar'
```


<div id="parse" class="anchor"></div>
## querystring.parse(str[, sep][, eq][, options])

将一个查询字符串反序列化为一个对象。可以选择是否覆盖默认的分割符（`'&'`）和分配符（`'='`）。

Options 对象可能包含 `maxKeys` 属性（默认为 1000），它会被用来限制已处理键（key）的数量。设为 0 可以去除键（key）的数量限制。

Options 对象可能包含 `decodeURIComponent` 属性（默认为 `querystring.unescape`）。在有必要时，它可以将 `non-utf8` 码解码为字符串。

```javascript
querystring.parse('foo=bar&baz=qux&baz=quux&corge')
// returns { foo: 'bar', baz: ['qux', 'quux'], corge: '' }

// Suppose gbkDecodeURIComponent function already exists,
// it can decode `gbk` encoding string
querystring.parse('w=%D6%D0%CE%C4&foo=bar', null, null,
  { decodeURIComponent: gbkDecodeURIComponent })
// returns { w: '中文', foo: 'bar' }
```


<div id="escape" class="anchor"></div>
## querystring.escape

供 `querystring.stringify` 使用的转意函数，在必要的时候可被重写。


<div id="unescape" class="anchor"></div>
## querystring.unescape

供 `querystring.parse` 使用的反转意函数，在必要的时候可被重写。

它会首先尝试使用 `encodeURIComponent`，但如果失败，它会回滚到一个安全的等效结果上，在处理畸形的 URL 时也不会抛出错误。