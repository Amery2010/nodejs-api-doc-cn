# 调试器(Debugger)

> 稳定度：2 - 稳定

Node.js 包含一个可以有效地通过 [TCP 协议](https://github.com/v8/v8/wiki/Debugging-Protocol) 访问的完整的进程外的全功能调试工具并内置调试客户端。在启动 Node.js 后，通过 `debug` 参数加上需要调试的脚本文件路径的方式使用，在调试器成功启动后会有明显的提示：

```
$ node debug myscript.js
< debugger listening on port 5858
connecting... ok
break in /home/indutny/Code/git/indutny/myscript.js:1
  1 x = 5;
  2 setTimeout(() => {
  3   debugger;
debug>
```

Node.js 的调试器客户端虽然目前还没法支持全部命令，但可以进行一些简单的（调试）步骤和检测（命令）。

在脚本的源代码中插入一个 `debugger;` 声明就可以在当前位置的代码中设置一个断点。

例如，假设 `myscript.js` 是这么写的：

```javascript
// myscript.js
x = 5;
setTimeout(() => {
    debugger;
    console.log('world');
}, 1000);
console.log('hello');
```

一旦运行调试器，将在第4行发生断点：

```
// myscript.js
x = 5;
setTimeout(() => {
  debugger;
  console.log('world');
}, 1000);
console.log('hello');
Once the debugger is run, a breakpoint will occur at line 4:

$ node debug myscript.js
< debugger listening on port 5858
connecting... ok
break in /home/indutny/Code/git/indutny/myscript.js:1
  1 x = 5;
  2 setTimeout(() => {
  3   debugger;
debug> cont
< hello
break in /home/indutny/Code/git/indutny/myscript.js:3
  1 x = 5;
  2 setTimeout(() => {
  3   debugger;
  4   console.log('world');
  5 }, 1000);
debug> next
break in /home/indutny/Code/git/indutny/myscript.js:4
  2 setTimeout(() => {
  3   debugger;
  4   console.log('world');
  5 }, 1000);
  6 console.log('hello');
debug> repl
Press Ctrl + C to leave debug repl
> x
5
> 2+2
4
debug> next
< world
break in /home/indutny/Code/git/indutny/myscript.js:5
  3   debugger;
  4   console.log('world');
  5 }, 1000);
  6 console.log('hello');
  7
debug> quit
```

`repl` 命令允许代码被远程评估。`next` 命令用于跳转到下一行。键入 `help` 可以查看其他的有效命令。