# REPL新特性

在 REPL 内，Control+D 将退出。可以输入多行表达式。全局变量和局部变量都支持使用 Tab 补全。核心模块将按需加载到环境中。例如，访问 `fs` 将 `require()` 该 `fs` 模块作为 `global.fs`。

特殊变量 `_`（下划线）包含最后一个表达式的结果。

``` bash
> [ 'a', 'b', 'c' ]
[ 'a', 'b', 'c' ]
> _.length
3
> _ += 1
4
```

REPL 提供了对全局作用域中任何变量的访问权限。你可以通过将其分配给与每个 `REPLServer` 关联的 `context` 对象来显式地将一个变量公开给 REPL。例如：

``` javascript
// repl_test.js
const repl = require('repl');
var msg = 'message';

repl.start('> ').context.m = msg;
```

`context` 对象中的变量在 REPL 中显示为本地变量：

``` bash
$ node repl_test.js
> m
'message'
```

有一些特殊的 REPL 命令：

* `.break` - 在输入多行表达式时，有时你会迷失或只是不在乎完成它。`.break` 会重来。

* `.clear` - 重置 `context` 对象为一个空对象，并清除任何的多行表达式。

* `.exit` - 关闭 `I/O` 流，这将导致 REPL 退出。

* `.help` - 显示这些特殊的命令列表。

* `.save` - 将当前的 REPL 会话保存到文件中。 `.save ./file/to/save.js`

* `.load` - 将文件加载到当前的 REPL 会话中。 `.load ./file/to/load.js`

REPL 中的以下组合键具有以下特殊效果：

* `<ctrl>C` - 类似于 `.break` 关键字。终止当前命令。在空白行上按两次可强制退出。

* `<ctrl>D` - 类似于 `.exit` 关键字。

* `<tab>` - 显示全局和局部（作用域）的变量。


## 显示在 REPL 中的自定义对象

当打印值时，REPL 模块在内部使用 [util.inspect()](../util/util.md#inspect)。然而，`util.inspect` 将调用委托给对象的 `inspect()` 函数，如果它有的话。你可以在[这里](../util/util.md#custom_inspect_function_on_Objects)阅读有关此委托的更多信息。

例如，如果你已经在一个对象上像这样定义了一个 `inspect()` 函数：

``` javascript
> var obj = {foo: 'this will not show up in the inspect() output'};
undefined
> obj.inspect = () => {
...   return {bar: 'baz'};
... };
[Function]
```

并试着在 REPL 中打印 `obj`，它将会调用自定义的 `inspect()` 函数：

``` bash
> obj
{bar: 'baz'}
```