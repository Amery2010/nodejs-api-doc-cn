# 异常与错误

JavaScript 异常通常是抛出一个无效操作的结果值或作为 `throw` 所表述的目标。虽然它不要求这些值是 `Error` 或继承自 `Error` 的类的实例，但会通过 Node.js 抛出所有异常或**将**成为 JavaScript 运行时的 `Error` 实例。

这些异常在 JavaScript 层是*无法恢复*的。这些异常总会引起 Node.js 进程的崩溃。这些例子包括 `assert()` 检测或在 C++ 层调用的 `abort()` 。