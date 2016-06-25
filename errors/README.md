# 错误(Errors)

在 Node.js 中运行的应用一般会遇到以下四类错误：

* 标准的 JavaScript 错误：
  - [EvalError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/EvalError)：当一个 `eval()` 回调失败时抛出。
  - [SyntaxError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)：当响应错误的 JavaScript 语法时抛出。
  - [RangeError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RangeError)：当一个值不在预期范围内时抛出。
  - [ReferenceError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)：当使用未定义的变量时抛出。
  - [TypeError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)：当传递错误类型的参数时抛出。
  - [URIError](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/URIError)：当一个全局的 URI 处理函数被误用时抛出。
  
* 由于底层操作系统的限制引发的系统错误。例如，试图打开不存在的文件，试图向一个已关闭的套接字（socket）发送数据等。

* 由应用程序代码触发的用户指定（User-specified）的错误。

* 每当 Node.js 检测到一个不应该发生得特殊逻辑冲突时，可以触发断言错误（Assertion Error）这一种特殊错误。这些错误通常由 `assert` 模块触发。

由 Node.js 提出的所有 JavaScript 和系统错误都实例化或继承自标准的 JavaScript [错误](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error)类，并*至少*保证是该类中的可用属性。