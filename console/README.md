# 控制台(Console)

* [Console 类](./class_Console.md)
* [异步与同步的控制台](./asynchronous_vs_synchronous_consoles.md)

----------------------------------------


> 稳定度：2 - 稳定

`console` 模块提供了一个简单的调试控制台，与 Web 浏览器提供的 JavaScript 控制台机制类似。

该模块导出两个特定组件：

* 一个 `Console` 类，包含类似于 `console.log()` 、 `console.error()` 和 `console.warn()` 这些方法，可以用于写入任何的 Node.js 流。

* 一个全局的 `console` 实例，用于写入 `stdout` 和 `stderr`。由于该对象是一个全局变量，它可以在没有调用 `require('console')` 的情况下使用。

使用全局 `console` 的示例：

``` javascript
console.log('hello world');
// 在 stdout 中打印: hello world
console.log('hello %s', 'world');
// 在 stdout 中打印: hello world
console.error(new Error('Whoops, something bad happened'));
// 在 stderr 中打印: [Error: Whoops, something bad happened]

const name = 'Will Robinson';
console.warn(`Danger ${name}! Danger!`);
// 在 stderr 中打印: Danger Will Robinson! Danger!
```

使用 `Console` 类的示例：

``` javascript
const out = getStreamSomehow();
const err = getStreamSomehow();
const myConsole = new console.Console(out, err);

myConsole.log('hello world');
// 在 stdout 中打印: hello world
myConsole.log('hello %s', 'world');
// 在 stdout 中打印: hello world
myConsole.error(new Error('Whoops, something bad happened'));
// 在 stderr 中打印: [Error: Whoops, something bad happened]

const name = 'Will Robinson';
myConsole.warn(`Danger ${name}! Danger!`);
// 在 stderr 中打印: Danger Will Robinson! Danger!
```

虽然 `Console` 类的 API 是根据浏览器的 `console` 对象设计的，但 Node.js 中的 `Console` 并没有完全复制浏览器中的功能。