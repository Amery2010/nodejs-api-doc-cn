# 示例：Tiny CLI

这里有一个如何使用所有这些接口一起来创建一个小型命令行界面的示例：

``` javascript
const readline = require('readline');
const rl = readline.createInterface(process.stdin, process.stdout);

rl.setPrompt('OHAI> ');
rl.prompt();

rl.on('line', (line) => {
    switch (line.trim()) {
        case 'hello':
            console.log('world!');
            break;
        default:
            console.log('Say what? I might have heard `' + line.trim() + '`');
            break;
    }
    rl.prompt();
}).on('close', () => {
    console.log('Have a great day!');
    process.exit(0);
});
```