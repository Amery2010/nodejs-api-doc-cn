# RangeError类

是 `Error` 的一个子类用以表示一个给定参数没有被内部设置或函数值不在可接受范围内；无论这是一个数字范围还是给定函数参数的选项之外的设置（原文，whether that is a numeric range, or outside the set of options for a given function parameter。理解了半天也没搞明白这句话到底想说明什么...[译者](https://github.com/Amery2010)注）。

例如：

```javascript
require('net').connect(-1);
// throws RangeError, port should be > 0 && < 65536
```

Node.js 会生成并以一种参数验证的形式**立即**抛出 `RangeError` 实例。