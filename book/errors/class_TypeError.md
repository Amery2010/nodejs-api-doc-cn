# TypeError类

是 `Error` 的一个子类用以表明所提供的参数不是一个被允许的类型。例如，将一个函数传递给一个需要字符串的参数时会产生一个 `TypeError` 。

```javascript
require('url').parse(function () {});
// throws TypeError, since it expected a string
```

Node.js 会生成并以一种参数验证的形式**立即**抛出 `TypeError` 实例。