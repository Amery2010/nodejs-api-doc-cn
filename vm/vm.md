# 方法和属性

* [vm.createContext([sandbox])](#createContext)
* [vm.isContext(sandbox)](#isContext)
* [vm.runInContext(code, contextifiedSandbox[, options])](#runInContext)
* [vm.runInNewContext(code[, sandbox][, options])](#runInNewContext)
* [vm.runInThisContext(code[, options])](#runInThisContext)
* [vm.runInDebugContext(code)](#runInDebugContext)

--------------------------------------------------


<div id="createContext" class="anchor"></div>
## vm.createContext([sandbox])

如果给一个 `sandbox` 的对象，则将沙箱 (sandbox) 对象 “上下文化 (contextify)” 供 [vm.runInContext](#runInContext) 或 [script.runInContext](./class_Script.md#runInContext) 调用。 以此方式运行的脚本将以 `sandbox` 作为全局对象，该对象将在保留其所有的属性的基础上拥有标准[全局对象](https://es5.github.io/#x15.1)所拥有的内置对象和函数。在由 vm 模块运行的脚本之外的地方 `sandbox` 将不会被改变。

如果没有提供沙箱对象，则返回一个新的、空的、没被上下文化的可用沙箱。

此函数可用于创建可执行多个脚本的沙箱，如，在模拟浏览器的时候可以使用该函数创建一个用于表示 `window` 全局对象的沙箱， 并将所有 `<script>` 标签放入沙箱执行。


<div id="isContext" class="anchor"></div>
## vm.isContext(sandbox)

返回一个沙箱对象是否通过调用 [vm.createContext()](#createContext) 使其上下文化。


<div id="runInContext" class="anchor"></div>
## vm.runInContext(code, contextifiedSandbox[, options])

`vm.runInContext()` 编译 `code`，然后在 `contextifiedSandbox` 中运行它并返回结果。运行的代码不能访问本地作用域。`contextifiedSandbox` 对象必须预先通过 [vm.createContext()](#createContext) 上下文化。它将被用作 `code` 的全局对象。

`vm.runInContext()` 与 [vm.runInThisContext()](#runInThisContext) 使用相同的选项。

例子：编译并在一个现有的上下文中执行不同的脚本。

``` javascript
const util = require('util');
const vm = require('vm');

const sandbox = { globalVar: 1 };
vm.createContext(sandbox);

for (var i = 0; i < 10; ++i) {
    vm.runInContext('globalVar *= 2;', sandbox);
}
console.log(util.inspect(sandbox));

// { globalVar: 1024 }
```


<div id="runInNewContext" class="anchor"></div>
## vm.runInNewContext(code[, sandbox][, options])

