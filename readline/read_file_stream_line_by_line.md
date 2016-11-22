# 示例：逐行读取文件流

一个常见的 `readline` 的 `input` 选项情况是将文件系统的可读流传递给它。这是一个如何处理文件逐行解析的例子：

``` javascript
const readline = require('readline');
const fs = require('fs');

const rl = readline.createInterface({
    input: fs.createReadStream('sample.txt')
});

rl.on('line', (line) => {
    console.log('Line from file:', line);
});
```