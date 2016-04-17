# 命令参考

* [步进](#stepping)
* [断点](#breakpoints)
* [信息](#info)
* [执行控制](#execution_control)
* [杂项](#various)

--------------------------------------------------


<div id="stepping" class="anchor"></div>
## 步进

* `cont`，`c` - 继续执行

* `next`，`n` - 下一步

* `step`，`s` - 介入

* `out`，`o` - 退出介入

* `pause` - 暂停执行代码（类似开发者工具中的暂停按钮）


<div id="breakpoints" class="anchor"></div>
## 断点

* `setBreakpoint()`，`sb()` - 在当前行设置断点

* `setBreakpoint(line)`，`sb(line)` - 在指定行设置断点

* `setBreakpoint('fn()')`，`sb(...)` - 在函数体的第一条语句设置断点

* `setBreakpoint('script.js', 1)`，`sb(...)` - 在 script.js 的第一行设置断点

* `clearBreakpoint('script.js', 1)`，`cb(...)` - 清除 script.js 第一行的断点

也可以在一个尚未被加载的文件（模块）中设置断点：

```
$ ./node debug test/fixtures/break-in-module/main.js
< debugger listening on port 5858
connecting to port 5858... ok
break in test/fixtures/break-in-module/main.js:1
  1 var mod = require('./mod.js');
  2 mod.hello();
  3 mod.hello();
debug> setBreakpoint('mod.js', 23)
Warning: script 'mod.js' was not loaded yet.
  1 var mod = require('./mod.js');
  2 mod.hello();
  3 mod.hello();
debug> c
break in test/fixtures/break-in-module/mod.js:23
 21
 22 exports.hello = () => {
 23   return 'hello from module';
 24 };
 25
debug>
```


<div id="info" class="anchor"></div>
## 信息

* `backtrace`，`bt` - 显示当前执行框架的回溯

* `list(5)` - 显示脚本源代码的 5 行上下文（之前 5 行和之后 5 行）

* `watch(expr)` - 向监视列表添加表达式

* `unwatch(expr)` - 从监视列表移除表达式

* `watchers` - 列出所有监视器和它们的值（每个断点会自动列出）

* `repl` - 在所调试的脚本的上下文中打开调试器的REPL进行评估

* `exec expr` - 在所调试的脚本的上下文中执行一个表达式


<div id="execution_control" class="anchor"></div>
## 执行控制

* `run` - 运行脚本（调试器开始时自动运行）

* `restart` - 重新启动脚本

* `kill` - 终止脚本


<div id="various" class="anchor"></div>
## 杂项

* `scripts` - 列出所有已加载的脚本

* `version` - 显示 V8 引擎的版本号