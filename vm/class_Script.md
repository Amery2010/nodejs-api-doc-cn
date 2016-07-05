# Script类

* [new vm.Script(code, options)](#new_vm_Script)
* [script.runInThisContext([options])](#runInThisContext)
* [script.runInContext(contextifiedSandbox[, options])](#runInContext)
* [script.runInNewContext([sandbox][, options])](#runInNewContext)

--------------------------------------------------


一个保存预编译脚本的类，并在特定的沙箱中运行它们。

<div id="new_vm_Script" class="anchor"></div>
## new vm.Script(code, options)

创建一个新的 `Script` 编译 `code`，但不运行它。相反，被创建的 `vm.Script` 对象用于表示此编译代码。此脚本可以在之后使用以下方法运行多次。返回的脚本不绑定到任何全局对象。它在每次运行前定向绑定。

创建脚本时的选项有：

* `filename`：允许你控制从该脚本中产生的任何堆栈跟踪中显示的文件名

* `lineOffset`：允许你添加一个显示在堆栈跟踪中行号的偏移量。

* `columnOffset`：允许你添加一个显示在堆栈跟踪中列数的偏移量。

* `displayErrors`：是否在抛出异常前输出带高亮错误代码行的错误信息到 stderr。仅适用于编译代码时的语法错误;而运行代码时的错误，通过脚本的方法选项来控制。

* `timeout`：在终止执行之前，执行 `code` 的毫秒数。如果执行被终止，[错误](../errors/class_Error.md#)将被抛出。

* `cachedData`：一个可选的 `Buffer` 和提供的源代码的 V8 代码缓存数据。当提供 `cachedDataRejected` 值时会根据由 V8 接受的数据设置 `true` 或 `false`。

* `produceCachedData`：如果为 `true` 并且不存在 `cachedData` - V8 试图生成代码缓存数据的 `code`。一旦成功，将产生和存储一个 `Buffer` 和 V8 的代码缓存数据到返回的 `vm.Script` 实例的缓存数据属性中。`cachedDataProduced` 值会被设置为 `true` 或 `false` 取决于代码缓存数据是否被成功产生。


<div id="runInThisContext" class="anchor"></div>
## script.runInThisContext([options])

类似于 [vm.runInThisContext()](./vm.md#runInThisContext) 但是一个预编译 `Script` 对象的方法。`script.runInThisContext()` 运行 `script` 的编译代码并返回结果。运行的代码无权限访问本地作用域，但是有权限访问当前的 `global` 对象。

使用 `script.runInThisContext()` 编译代码一次，运行多次：

``` javascript
const vm = require('vm');

global.globalVar = 0;

const script = new vm.Script('globalVar += 1', { filename: 'myfile.vm' });

for (var i = 0; i < 1000; ++i) {
    script.runInThisContext();
}

console.log(globalVar);

// 1000
```

运行脚本的选项有：

* `filename`：允许你控制产生在任何堆栈跟踪中显示的文件名。

* `lineOffset`：允许你添加一个显示在堆栈跟踪中行号的偏移量。

* `columnOffset`：允许你添加一个显示在堆栈跟踪中列数的偏移量。

* `displayErrors`：是否在抛出异常前输出带高亮错误代码行的错误信息到 stderr。将捕捉从编译 `code` 时获得的语法错误和通过执行编译代码抛出的运行时错误。仅适用于执行代码的运行时错误；它不可能创建一个语法错误的 `Script` 实例作为构造函数抛出。

* `timeout`：在终止执行之前，执行 `code` 的毫秒数。如果执行被终止，[错误](../errors/class_Error.md#)将被抛出。


<div id="runInContext" class="anchor"></div>
## script.runInContext(contextifiedSandbox[, options])

类似于 [vm.runInContext()](./vm.md#runInContext) 但是一个预编译 `Script` 对象的方法。`script.runInThisContext()` 在 `contextifiedSandbox` 中运行 `script` 的编译代码并返回结果。运行的代码无权限访问本地作用域。

`script.runInContext()` 与 [script.runInThisContext()](#runInThisContext) 使用相同的选项。

例子：编译递增一个全局变量，并设置一个变量，然后执行该代码多次。这些全局变量包含在沙箱中。

``` javascript
const util = require('util');
const vm = require('vm');

var sandbox = {
    animal: 'cat',
    count: 2
};

var context = new vm.createContext(sandbox);
var script = new vm.Script('count += 1; name = "kitty"');

for (var i = 0; i < 10; ++i) {
    script.runInContext(context);
}

console.log(util.inspect(sandbox));

// { animal: 'cat', count: 12, name: 'kitty' }
```

请注意，运行不受信任的代码是一个棘手的事情，需要非常小心。`script.runInContext()` 非常有用，但安全运行不受信任的代码需要一个单独的进程。


<div id="runInNewContext" class="anchor"></div>
## script.runInNewContext([sandbox][, options])

类似于 [vm.runInNewContext()](./vm.md#runInNewContext) 但是一个预编译 `Script` 对象的方法。如果传入 `sandbox`，则使用 `script.runInNewContext()` 将其上下文化，然后运行 `script` 的编译代码将沙箱作为全局对象并返回结果。运行的代码无权限访问本地作用域。

`script.runInNewContext()` 与 [script.runInThisContext()](#runInThisContext) 使用相同的选项。

例子：编译代码，设置一个全局变量，然后在不同的上下文中多次执行代码。这些全局变量在沙箱内部被设置。

``` javascript
const util = require('util');
const vm = require('vm');

const sandboxes = [{}, {}, {}];

const script = new vm.Script('globalVar = "set"');

sandboxes.forEach((sandbox) => {
    script.runInNewContext(sandbox);
});

console.log(util.inspect(sandboxes));

// [{ globalVar: 'set' }, { globalVar: 'set' }, { globalVar: 'set' }]
```

请注意，运行不受信任的代码是一个棘手的事情，需要非常小心。`script.runInNewContext()` 非常有用，但安全运行不受信任的代码需要一个单独的进程。