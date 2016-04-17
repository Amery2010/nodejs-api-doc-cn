# 错误的冒泡和捕捉

* [Node.js 风格的回调](#node_js_style_callbacks)

--------------------------------------------------


Node.js 支持几种在程序运行时发生的错误冒泡和错误处理机制。如何报告和处理这些错误完全取决于 `Error` 的类型和被调用的 API 的风格。

所有的 JavaScript 错误都是作为异常处理的，*立即*产生并通过标准的 JavaScript `throw` 机制抛出错误。这些都是利用 JavaScript 语言提供的 [try / catch construct](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch) 处理的。

```javascript
// Throws with a ReferenceError because z is undefined
try {
    const m = 1;
    const n = m + z;
} catch (err) {
    // Handle the error here.
}
```

JavaScript 的 `throw` 机制在任何时候使用都会引发异常，*必须*通过 `try / catch` 处理，否则 Node.js 会立即退出进程。

但也有少数例外，*同步*的 API（任何不接受 `callback` 函数的阻塞方法，例如，[fs.readFileSync](../fs/fs.md#readFileSync)）会使用 `throw` 报告错误。

异步的 API 中发生的错误可能会以多种方式进行报告：

* 大多数的异步方法都接受一个 `callback` 函数，该函数接受给其第一个参数传递一个 `Error` 对象。如果第一个参数不是 `null` 或是一个 `Error` 实例，就应该对发生的错误进行处理。

```javascript
const fs = require('fs');
fs.readFile('a file that does not exist', (err, data) => {
    if (err) {
        console.error('There was an error reading the file!', err);
        return;
    }
    // Otherwise handle the data
});
```

* 当 `EventEmitter` 对象调用一个异步方法时，错误会被分发到该对象的 `'error'` 事件上。

```javascript
const net = require('net');
const connection = net.connect('localhost');

// Adding an 'error' event handler to a stream:
connection.on('error', (err) => {
    // If the connection is reset by the server, or if it can't
    // connect at all, or on any sort of error encountered by
    // the connection, the error will be sent here.
    console.error(err);
});

connection.pipe(process.stdout);
```

* 在 Node.js API 中有一小部分普通的异步方法仍可能使用 `throw` 机制引发错误，必须使用 `try / catch` 处理。这些方法并没有一个完整的列表。请参阅各类方法的文档以确定所需的合适的错误处理机制。

大多数的 [stream-based](../stream/) 和 [event emitter-based](../events/class_EventEmitter.md#) API 都使用相同的 `'error'` 事件机制，它们本身就代表了一系列随着时间推移的异步操作（相对于单一操作，可能有效也可能无效）。

对于*所有*的 `EventEmitter` 对象而言，如果不能提供一个 `'error'` 事件处理程序，那么错误将被抛出，从而导致 Node.js 进程报告一个未处理的异常并随即崩溃，除非适当的使用 [域](../domain/) 模块或已经注册了 [process.on('uncaughtException')](../process/process.md#uncaughtException) 事件。

```javascript
const EventEmitter = require('events');
const ee = new EventEmitter();

setImmediate(() => {
    // This will crash the process because no 'error' event
    // handler has been added.
    ee.emit('error', new Error('This will crash'));
});
```

在调用的代码退出后抛出的错误，不能通过 `try / catch` 截获。

开发者必须查阅各类方法的文档以明确在错误发生时这些方法是如何冒泡的。


<div id="node_js_style_callbacks" class="anchor"></div>
## Node.js 风格的回调

大多数由 Node.js 核心 API 暴露出来的异步方法都遵循称之为“Node.js 风格的回调”的惯用模式。通过这种模式，可以用作为参数的方法传递函数。当操作完成或引发错误时，`Error` 对象（无论如何）都会作为第一个参数被回调函数所调用。如果没有引发错误，第一个参数会作为 `null` 传递。

```javascript
const fs = require('fs');

function nodeStyleCallback(err, data) {
    if (err) {
        console.error('There was an error', err);
        return;
    }
    console.log(data);
}

fs.readFile('/some/file/that/does-not-exist', nodeStyleCallback);
fs.readFile('/some/file/that/does-exist', nodeStyleCallback)
```

JavaScript 的 `try / catch` 机制**不应该**用于截取由异步 API 引起的错误。尝试使用 `throw` 替代一个 Node.js 风格的回调是一个初学者常犯的错误：

```javascript
// THIS WILL NOT WORK:
const fs = require('fs');

try {
    fs.readFile('/some/file/that/does-not-exist', (err, data) => {
        // mistaken assumption: throwing here...
        if (err) {
            throw err;
        }
    });
} catch (err) {
    // This will not catch the throw!
    console.log(err);
}
```

这并没有什么用，因为 `fs.readFile()` 是一个异步调用的回调函数。当回调函数被调用时，这些代码（包括 `try { } catch(err) { }` 区域）就已经退出。在大对数案例中，抛出回调函数中的错误会**引起 Node.js 进程崩溃**。如果[域](../domain/)被启用，或已注册了 [process.on('uncaughtException')](../process/process.md#uncaughtException) 事件，那么这样的错误是可以被拦截的。