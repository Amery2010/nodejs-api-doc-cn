# 逐行读取(Readline)

> 稳定度：2 - 稳定

通过 `require('readline')` 使用该模块。Readline 允许在逐行的基础上读取流（例如 [process.stdin](../process/process.md#stdin)）。

请注意，一旦你调用此模块，直到你关闭界面前，你的 Node.js 程序将不会终止。以下代码是如何允许你的程序正常退出：

``` javascript
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.question('What do you think of Node.js? ', (answer) => {
    // TODO: Log the answer in a database
    console.log('Thank you for your valuable feedback:', answer);

    rl.close();
});
```