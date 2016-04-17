# 控制台(Console)

> 稳定度：2 - 稳定

`console` 模块提供了一个简单的调试控制台，类似于Web浏览器中的 Javascript 控制台机制。

该模块导出两个特定组件：

* 一个 `Console` 类，包含类似于 `console.log()` 、 `console.error()` 和 `console.warn()` 这些方法，它可以用于任何的 Node.js 流中。


* 一个全局的 `console` 用于打印 `stdout` 和 `stderr` 。由于该对象是一个全局变量，它可以在没用 `require('console')` 的情况下使用。

举个使用全局 `console` 的栗子：

``` javascript
console.log('hello world');
  // 打印 hello world 的标准输出
console.log('hello %s', 'world');
  // 打印 hello world 的标准输出
console.error(new Error('Whoops, something bad happened'));
  // 打印 [Error: Whoops, something bad happened] 的标准错误

const name = 'Will Robinson';
console.warn(`Danger ${name}! Danger!`);
  // 打印 Danger Will Robinson! Danger! 的标准错误
```

举个使用 `Console` 类的栗子：

``` javascript
const out = getStreamSomehow();
const err = getStreamSomehow();
const myConsole = new console.Console(out, err);

myConsole.log('hello world');
  // 打印 hello world 的标准输出
myConsole.log('hello %s', 'world');
  // 打印 hello world 的标准输出
myConsole.error(new Error('Whoops, something bad happened'));
  // 打印 [Error: Whoops, something bad happened] 的标准错误

const name = 'Will Robinson';
myConsole.warn(`Danger ${name}! Danger!`);
  // 打印 Danger Will Robinson! Danger! 的标准错误
```

`Console` 类的API是根据浏览器的 `console` 对象设计的，但 Node.js 中的 `Console` 并不是为了精准地复制浏览器功能。