# Error类

* [new Error(message)](#new_Error)
* [Error.captureStackTrace(targetObject[, constructorOpt])](#captureStackTrace)
* [Error.stackTraceLimit](#stackTraceLimit)
  - [error.message](#message)
  - [error.stack](#stack)
  
--------------------------------------------------


一个普通的 JavaScript `Error` 对象不会表述错误发生的具体原因。`Error` 对象会捕捉一个详细说明被实例化的 `Error` 对象在代码中的位置的“堆栈跟踪”，并可能提供对此错误的文字描述。

所有由 Node.js 产生的系统和 JavaScript 错误都继承实例化或继承自 `Error` 类。


<div id="new_Error" class="anchor"></div>
## new Error(message)

新建一个 `Error` 实例并设置它的 `error.message` 属性以提供文本信息。如果 `message` 传的是一个对象，则会通过 `message.toString()` 生成文本信息。`error.stack` 属性会表明 `new Error()` 在代码中的位置。
堆栈跟踪是根据 [V8 的堆栈跟踪 API](https://github.com/v8/v8/wiki/Stack-Trace-API) 生成的。堆栈跟踪只会取（a）*异步代码开始执行*前或（b）`Error.stackTraceLimit` 属性给出的栈帧中的最小项。


<div id="captureStackTrace" class="anchor"></div>
## Error.captureStackTrace(targetObject[, constructorOpt])

当访问调用 `Error.captureStackTrace()` 方法所返回的表示代码中的位置的字符串时，会在 `targetObject` 上创建一个 `.stack` 属性。

```javascript
const myObject = {};
Error.captureStackTrace(myObject);
myObject.stack  // similar to `new Error().stack`
```

堆栈跟踪的第一行会是前缀被替换成 `ErrorType: message` 的 `targetObject.toString()` 的调用结果。

可选的 `constructorOpt` 接受一个函数作为其参数。如果提供了该参数，则 `constructorOpt` 以前包括自身在内的全部栈帧都会被其生成的堆栈跟踪省略。

`constructorOpt` 用在向最终用户隐藏错误生成的具体细节时非常有用。例如：

```javascript
function MyError() {
    Error.captureStackTrace(this, MyError);
}

// Without passing MyError to captureStackTrace, the MyError
// frame would should up in the .stack property. by passing
// the constructor, we omit that frame and all frames above it.
new MyError().stack
```


<div id="stackTraceLimit" class="anchor"></div>
## Error.stackTraceLimit

`Error.stackTraceLimit` 属性用于指定堆栈跟踪所收集的栈帧数量（无论是由 `new Error().stack` 还是由 `Error.captureStackTrace(obj)` 产生的）。

默认值为 `10` ，但可以设置成任何有效 JavaScript 数值。这个修改会影响到*值被改变后*捕捉到的所有堆栈跟踪。

如果设置成一个非数值或负数，堆栈跟踪将不会捕捉任何栈帧。


<div id="message" class="anchor"></div>
#### error.message

返回由 `new Error(message)` 设置的用来描述错误的字符串。这个传递给构造函数的 `message` 也将出现在 `Error` 的堆栈跟踪的第一行。然而，在 `Error` 对象创建后改变这个属性*可能不会*改变堆栈跟踪的第一行。

```javascript
const err = new Error('The message');
console.log(err.message);
// Prints: The message
```


<div id="stack" class="anchor"></div>
#### error.stack

返回一个描述 `Error` 实例在代码中的位置的字符串。

例如：

```
Error: Things keep happening!
   at /home/gbusey/file.js:525:2
   at Frobnicator.refrobulate (/home/gbusey/business-logic.js:424:21)
   at Actor.<anonymous> (/home/gbusey/actors.js:400:8)
   at increaseSynergy (/home/gbusey/actors.js:701:6)
```

第一行会被格式化为 `<error class name>: <error message>` ，并且随后跟着一系列栈帧（每一行都会以 "at " 开头）。每一帧都描述了一个代码中导致错误生成的调用点。V8 引擎会尝试显示每个函数的名称（变量名、函数名或对象的方法名），但偶尔也可能找不到一个合适的名称。如果 V8 引擎没法确定一个函数的名称，在该栈帧中只会显示仅有的位置信息。否则，在位置信息的附近将会显示已确定的函数名。

注意，这对**仅**由 JavaScript 函数产生的栈帧非常有用。例如，在自身是一个 JavaScript 函数的情况下，通过同步执行一个名为 `cheetahify` 的 C++ 插件时，代表 `cheetahify` 回调的栈帧将**不会**出现在当前的堆栈跟踪里：


```javascript
const cheetahify = require('./native-binding.node');

function makeFaster() {
    // cheetahify *synchronously* calls speedy.
    cheetahify(function speedy() {
        throw new Error('oh no!');
    });
}

makeFaster(); // will throw:
// /home/gbusey/file.js:6
//     throw new Error('oh no!');
//           ^
// Error: oh no!
//     at speedy (/home/gbusey/file.js:6:11)
//     at makeFaster (/home/gbusey/file.js:5:3)
//     at Object.<anonymous> (/home/gbusey/file.js:10:1)
//     at Module._compile (module.js:456:26)
//     at Object.Module._extensions..js (module.js:474:10)
//     at Module.load (module.js:356:32)
//     at Function.Module._load (module.js:312:12)
//     at Function.Module.runMain (module.js:497:10)
//     at startup (node.js:119:16)
//     at node.js:906:3
```

其中的位置信息会以下形式出现：

* `native`，如果栈帧产生自 V8 引擎内部（比如，`[].forEach`）。

* `plain-filename.js:line:column`，如果栈帧产生自 Node.js 内部。

* `/absolute/path/to/file.js:line:column`，如果栈帧产生自用户程序或其依赖。

代表堆栈跟踪的字符串是在**访问** `error.stack` 属性时才被生成的。

堆栈跟踪捕获的栈帧的数量是由 `Error.stackTraceLimit` 或当前事件循环可用的栈帧数量中的最小值界定。

系统级的错误由已传参的 `Error` 实例产生，详见[此处](./system_errors.md#)。