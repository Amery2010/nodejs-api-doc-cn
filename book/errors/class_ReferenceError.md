# ReferenceError类

是 `Error` 的一个子类用以表示企图访问一个未定义的变量。这些错误通常表示代码中的错别字或一个坏掉的程序。虽然客户端代码可能会产生和传播这些错误，但在实践中，只有 V8 引擎会这么做。

```javascript
doesNotExist;
// throws ReferenceError, doesNotExist is not a variable in this program.
```

`ReferenceError` 实例会有一个 `error.arguments` 属性，其值为一个只有单个元素（一个代表变量未定义的字符串）的数组。

```javascript
const assert = require('assert');
try {
    doesNotExist;
} catch (err) {
    assert(err.arguments[0], 'doesNotExist');
}
```

除非一个应用程序是动态生成并运行的代码，否则 `ReferenceError` 实例会始终被视为在代码或其依赖中的错误（bug）。