# Hello world

* [构建](#building)
* [链接到 Node.js 自己的依赖](linking_to_nodejs_own_dependencies)
* [使用 require() 加载插件](loading_addons)

--------------------------------------------------


这个“Hello World”示例是一个简单的插件，用 C++ 编写，这等同于如下的 JavaScript代码：

```javascript
module.exports.hello = () => 'world';
```

首先创建 `hello.cc` 文件：

```c++
// hello.cc
#include <node.h>

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void Method(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  args.GetReturnValue().Set(String::NewFromUtf8(isolate, "world"));
}

void init(Local<Object> exports) {
  NODE_SET_METHOD(exports, "hello", Method);
}

NODE_MODULE(addon, init)

}  // namespace demo
```

请注意，所有的 Node.js 插件必须导出如下格式的初始化函数：


```c++
void Initialize(Local<Object> exports);
NODE_MODULE(module_name, Initialize)
```

这里在 `NODE_MODULE` 后面没有分号，以表示它不是函数（详见 `node.h`）。

`module_name` 必须匹配最终二进制的文件名（不包括 .node 后缀）。

在 `hello.cc` 例子中，那么，`init` 是初始化函数，并且 `addon` 是插件模块的名称。


<div id="building" class="anchor"></div>
### 构建

一旦源代码已被写入，它必须被编译成二进制 `addon.node` 文件。要做到这一点，在项目的顶层使用类 JSON 的格式描述你的模块构建配置，用于创建一个叫做 `binding.gyp` 的文件。该文件通过 [node-gyp](https://github.com/nodejs/node-gyp) 使用 —— 专门写了一个用于编译 Node.js 插件的工具。

```
{
    "targets": [
        {
            "target_name": "addon",
            "sources": ["hello.cc"]
        }
    ]
}
```

*注意：一个实用的 `node-gyp` 版本作为 `npm` 的一部分，通过 Node.js 捆绑和发行。此版本没有做出可以直接供开发者使用的版本，目的只是为了支持使用 `npm install` 命令编译并安装插件的能力。希望直接使用 `node-gyp` 的开发者可以使用 `npm install -g node-gyp` 命令进行安装。查阅 `node-gyp` [安装说明](https://github.com/nodejs/node-gyp#installation)了解更多详情，包括平台特定的要求。*

一旦 `binding.gyp` 文件被创建，使用 `node-gyp configure` 为当前平台生成相应项目的构建文件。这会在 `build/` 目录下生成一个 `Makefile` (在 Unix 平台上)或 `vcxproj` 文件（在 Windows 上）。

下一步，调用 `node-gyp build` 命令生成 `addon.node` 的编译文件。这会被放到 `build/Release/` 目录下。

当使用 `npm install` 安装一个 Node.js 插件时，npm 使用自身捆绑的 `node-gyp` 版本来执行同样的一套动作，为用户的需要的平台产生一个插件的编译版本。

一旦构建完成，二进制插件就可以通过 `require()` 指向内置的 `addon.node` 模块在 Node.js 内部使用：

```javascript
// hello.js
const addon = require('./build/Release/addon');

console.log(addon.hello()); // 'world'
```

请参阅下面例子获取详细信息或在 [https://github.com/arturadib/node-qt](https://github.com/arturadib/node-qt) 了解关于生产环境中的例子。

因为编译后的二进制插件的确切路径可能取决于它是如何编译的（如，有时它可能在 `./build/Debug/`），插件可以使用[绑定](https://github.com/TooTallNate/node-bindings)包加载已编译的模块。

请注意，虽然 `bindings` 包在如何定位插件模块的实现上更精细，但它本质上是使用类似于一个 `try-catch` 的模式：

```javascript
try {
    return require('./build/Release/addon.node');
} catch (err) {
    return require('./build/Debug/addon.node');
}
```


<div id="linking_to_nodejs_own_dependencies" class="anchor"></div>
### 链接到 Node.js 自己的依赖

Node.js 使用了一些静态链接库，比如 V8 引擎、libuv 和 OpenSSL。所有的插件都需要链接到 V8，并且也可能链接到任何其他的依赖。通常情况下，只要简单的包含相应的 `#include <...>` 声明（如，`#include <v8.h>`），并且 `node-gyp` 会找到适当的头。不过，也有一些注意事项需要注意：

* 当 `node-gyp` 运行时，它会检测 Node.js 的具体发行版本，并下载完整源码包，或只是头。如果下载了全部的源码，插件将会有完整的权限去访问 Node.js 的全套依赖。然而，如果只下载了 Node.js 的头，则仅由 Node.js 导出的标记可用。

* `node-gyp` 可以使用指向本地 Node.js 源代码信息的 `--nodedir` 标识来运行。使用此选项，插件将有机会访问全套依赖。


<div id="loading_addons" class="anchor"></div>
### 使用 require() 加载插件

编译后的二进制插件的文件扩展名是 `.node`（而不是 `.dll` 或 `.so`）。[require()](../globals/global.md#require) 函数是被用于查找具有 `.node` 文件扩展名的文件，并初始化这些作为动态链接库。

当调用 [require()](../globals/global.md#require) 时，`.node` 拓展名通常是可以省略的，Node.js 仍可以发现并初始化该插件。警告，然而，Node.js 会首先尝试查找并加载模块或碰巧共享相同的基本名称的 JavaScript 文件。例如，如果有一个 `addon.js` 文件与二进制 `addon.node`在同一目录下，那么 [require('addon')](../globals/global.md#require) 将优先考虑 `addon.js` 文件并加载它来代替 `addon.node`。