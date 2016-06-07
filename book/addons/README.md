# C/C++插件(C/C++ Addons)

Node.js 插件是动态链接共享对象，用 C 或 C++ 编写，它可以使用 [require()](../globals/global.md#require) 函数加载到 Node.js 中，并且可以把它们当成普通的 Node.js 模块那样使用。它们主要用于提供 Node.js 和 C/C++ 库之间运行 JavaScript 的接口。

目前，用于实现插件的方法相当复杂，涉及多个组件和 API 的知识：

* V8：Node.js 中目前用于提供 JavaScript 实现的 C++ 库。V8 提供了用于创建对象，调用函数等机制。V8 的  API 大部分记录在 `v8.h` 头文件（在 Node.js 的源代码树中的 `deps/v8/include/v8.h`）中，也可以在[网上](https://v8docs.nodesource.com/)找到。

* [libuv](https://github.com/libuv/libuv)：实现了 Node.js 事件循环、工作线程和和所有的平台的异步行为的 C 库。它也可作为一个跨平台的抽象库，使得在所有的主流操作系统中可以像 POSIX 那样访问许多常见的系统任务，如与文件系统、sockets、计时器和系统事件的交互变得容易。libuv 还提供了一个可被用于为更复杂的异步插件供能的类 pthreads 的抽象线程，这需要高于标准的事件循环。鼓励插件作者去思考如何通过 libuv 的无阻塞系统操作，工作线程或自定义使用 libuv 线程在 I/O 或其他时间密集型任务中降低工作负载来避免阻塞事件循环。

* 内置的 Node.js 库。Node.js 自己导出我们可以使用的一些 C/C++ 的 API —— 其中最重要的是 `node::ObjectWrap` 类。

* Node.js 包括一些其他的静态链接库，包括 OpenSSL。这些其他的库位于 Node.js 源代码树中的 `deps/` 目录。只有 V8 和 OpenSSL 标志（symbol）有目的地通过 Node.js 再导出，并且可以通过插件进行不同程度的使用。查阅[链接到 Node.js 自己的依赖](./hello_world.md#linking_to_nodejs_own_dependencies)了解附加信息。

本章节中的所有实例都可以[下载](https://github.com/nodejs/node-addon-examples)，并可以作为你自己的插件的起始点。