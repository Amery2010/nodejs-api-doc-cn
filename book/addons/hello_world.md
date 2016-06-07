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