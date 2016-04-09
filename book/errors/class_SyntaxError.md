# SyntaxError类

是 `Error` 的一个子类用于表示当前程序不是有效的 JavaScript 代码。这些错误只会产生和传播代码的评测结果。代码评测可能产生自 `eval`、`Function`、`require` 或 [vm](../vm/)。这些错误几乎都表示这是一个坏掉的程序。

```javascript
try {
    require('vm').runInThisContext('binary ! isNotOk');
} catch (err) {
    // err will be a SyntaxError
}
```

`SyntaxError` 实例在创建它们的上下文中是不可恢复的 - 它们只可能被其他上下文捕获。