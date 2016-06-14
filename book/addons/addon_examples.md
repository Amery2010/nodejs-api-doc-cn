# 插件实例

* [函数的参数](#function_arguments)
* [回调](#callbacks)
* [对象工厂](#object_factory)
* [函数工厂](#function_factory)
* [包装 C++ 对象](#wrapping_C_plus_plus_objects)
* [包装对象的工厂](#factory_of_wrapped_objects)
* [传递包装的对象](#passing_wrapped_objects_around)
* [AtExit 挂钩](#AtExit_hooks)
  - [void AtExit(callback, args)](#AtExit)

--------------------------------------------------


以下是旨在帮助开发人员入门的一些插件示例。这些例子利用了 V8 的 API。请参阅在线 [V8 参考](https://v8docs.nodesource.com/)有助于了解各种 V8 回调和解释 [V8 的嵌入者指南](https://developers.google.com/v8/embed)中使用的几个概念，如处理器、作用域和函数模板等。

每个示例都使用以下的 `binding.gyp` 文件：

```c++
{
    "targets": [
        {
            "target_name": "addon",
            "sources": ["addon.cc"]
        }
    ]
}
```

在有一个以上的 `.cc` 文件的情况下，只要将额外的文件名添加到源数组。例如：

```c++
"sources": ["addon.cc", "myexample.cc"]
```

一旦 `binding.gyp` 文件准备就绪，示例中的插件就可以使用 `node-gyp` 进行配置和构建：

```
$ node-gyp configure build
```


<div id="function_arguments" class="anchor"></div>
## 函数的参数

插件通常会暴露可以从运行在 Node.js 中的 JavaScript 访问到的对象和函数。当函数在 JavaScript 中调用时，输入参数和返回值必须与 C/C++ 代码相互映射。

下面的例子说明了如何读取从 JavaScript 传递过来的函数参数和如何返回结果：

```c++
// addon.cc
#include <node.h>

namespace demo {

using v8::Exception;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Number;
using v8::Object;
using v8::String;
using v8::Value;

// This is the implementation of the "add" method
// Input arguments are passed using the
// const FunctionCallbackInfo<Value>& args struct
void Add(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  // Check the number of arguments passed.
  if (args.Length() < 2) {
    // Throw an Error that is passed back to JavaScript
    isolate->ThrowException(Exception::TypeError(
        String::NewFromUtf8(isolate, "Wrong number of arguments")));
    return;
  }

  // Check the argument types
  if (!args[0]->IsNumber() || !args[1]->IsNumber()) {
    isolate->ThrowException(Exception::TypeError(
        String::NewFromUtf8(isolate, "Wrong arguments")));
    return;
  }

  // Perform the operation
  double value = args[0]->NumberValue() + args[1]->NumberValue();
  Local<Number> num = Number::New(isolate, value);

  // Set the return value (using the passed in
  // FunctionCallbackInfo<Value>&)
  args.GetReturnValue().Set(num);
}

void Init(Local<Object> exports) {
  NODE_SET_METHOD(exports, "add", Add);
}

NODE_MODULE(addon, Init)

}  // namespace demo
```

一旦编译，示例插件就可以在 Node.js 中加载和使用：

```javascript
// test.js
const addon = require('./build/Release/addon');

console.log('This should be eight:', addon.add(3, 5));
```


<div id="callbacks" class="anchor"></div>
## 回调

通过传递 JavaScript 函数到一个 C++ 函数并在那里执行它们，这在插件里是常见的做法。下面的例子说明了如何调用这些回调：

```c++
// addon.cc
#include <node.h>

namespace demo {

using v8::Function;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Null;
using v8::Object;
using v8::String;
using v8::Value;

void RunCallback(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  Local<Function> cb = Local<Function>::Cast(args[0]);
  const unsigned argc = 1;
  Local<Value> argv[argc] = { String::NewFromUtf8(isolate, "hello world") };
  cb->Call(Null(isolate), argc, argv);
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", RunCallback);
}

NODE_MODULE(addon, Init)

}  // namespace demo
```

注意，这个例子的 `Init()` 使用两个参数，它接收完整的 `module` 对象作为第二个参数。这使得插件完全覆写 `exports` 以一个单一的函数代替添加函数作为 `exports` 的属性。

为了验证这一情况，请运行下面的 JavaScript：

```javascript
// test.js
const addon = require('./build/Release/addon');

addon((msg) => {
    console.log(msg); // 'hello world'
});
```

需要注意的是，在这个例子中，回调函数是同步调用的。