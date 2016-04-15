# Buffers和ES6迭代器

Buffers 可以通过 ECMAScript 2015 (ES6) 的 `for..of` 语法进行遍历。

```javascript
const buf = Buffer(.from[1, 2, 3]);

for (var b of buf)
    console.log(b)

// Prints:
//   1
//   2
//   3
```

另外，`buf.values()` 、`buf.keys()` 和 `buf.entries()` 方法可用于创建迭代器。