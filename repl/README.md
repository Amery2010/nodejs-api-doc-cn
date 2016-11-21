# 命令行交互(REPL)

> 稳定度：2 - 稳定

Read-Eval-Print-Loop（REPL）既可作为独立程序使用，也可轻松包含在其他程序中。REPL 提供了一种交互式运行 JavaScript 并查看结果的方法。它可以用于调试、测试或只是尝试新东西。

通过从命令行执行没有任何参数的 `node`，你将被放入 REPL。它有简单的 emacs 行编辑。

``` bash
$ node
Type '.help' for options.
> a = [ 1, 2, 3];
[ 1, 2, 3 ]
> a.forEach((v) => {
...   console.log(v);
...   });
1
2
3
```

对于高级线性编辑器，使用环境变量 `NODE_NO_READLINE=1` 启动 Node.js。在规范终端设置中启动主要的和调试的 REPL，这将允许你使用 `rlwrap`。

例如，你可以将其添加到你的 `bashrc` 文件中：

``` bash
alias node="env NODE_NO_READLINE=1 rlwrap node"
```