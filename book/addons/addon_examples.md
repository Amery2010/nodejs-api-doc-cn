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


<div id="object_factory" class="anchor"></div>
## 对象工厂

插件从如下示例中所示的 C++ 函数中创建和返回新对象。一个带有 `msg` 属性的对象被创建和返回，该属性是传递给 `createObject()` 的相呼应的字符串：

```c++
// addon.cc
#include <node.h>

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void CreateObject(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  Local<Object> obj = Object::New(isolate);
  obj->Set(String::NewFromUtf8(isolate, "msg"), args[0]->ToString());

  args.GetReturnValue().Set(obj);
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", CreateObject);
}

NODE_MODULE(addon, Init)

}  // namespace demo
```

在 JavaScript 中测试它：

```javascript
// test.js
const addon = require('./build/Release/addon');

var obj1 = addon('hello');
var obj2 = addon('world');
console.log(obj1.msg + ' ' + obj2.msg); // 'hello world'
```


<div id="function_factory" class="anchor"></div>
## 函数工厂

另一种常见情况是创建 JavaScript 函数来包装 C++ 函数，并返回那些使其回到 JavaScript：

```c++
// addon.cc
#include <node.h>

namespace demo {

using v8::Function;
using v8::FunctionCallbackInfo;
using v8::FunctionTemplate;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void MyFunction(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  args.GetReturnValue().Set(String::NewFromUtf8(isolate, "hello world"));
}

void CreateFunction(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, MyFunction);
  Local<Function> fn = tpl->GetFunction();

  // omit this to make it anonymous
  fn->SetName(String::NewFromUtf8(isolate, "theFunction"));

  args.GetReturnValue().Set(fn);
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", CreateFunction);
}

NODE_MODULE(addon, Init)

}  // namespace demo
```

去测试：

```javascript
// test.js
const addon = require('./build/Release/addon');

var fn = addon();
console.log(fn()); // 'hello world'
```


<div id="wrapping_C_plus_plus_objects" class="anchor"></div>
## 包装 C++ 对象

另外，也可以包装 C++ 对象/类允许以使用 JavaScript 的 `new` 操作的方式来创建新的实例：

```c++
// addon.cc
#include <node.h>
#include "myobject.h"

namespace demo {

using v8::Local;
using v8::Object;

void InitAll(Local<Object> exports) {
  MyObject::Init(exports);
}

NODE_MODULE(addon, InitAll)

}  // namespace demo
```

那么，在 `myobject.h` 中，包装类继承自 `node::ObjectWrap`：

```c++
// myobject.h
#ifndef MYOBJECT_H
#define MYOBJECT_H

#include <node.h>
#include <node_object_wrap.h>

namespace demo {

class MyObject : public node::ObjectWrap {
 public:
  static void Init(v8::Local<v8::Object> exports);

 private:
  explicit MyObject(double value = 0);
  ~MyObject();

  static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
  static void PlusOne(const v8::FunctionCallbackInfo<v8::Value>& args);
  static v8::Persistent<v8::Function> constructor;
  double value_;
};

}  // namespace demo

#endif
```

在 `myobject.cc` 中，实现要被暴露的各种方法。下面，`plusOne()` 方法是通过将其添加到构造函数的原型来暴露的：

```c++
// myobject.cc
#include "myobject.h"

namespace demo {

using v8::Function;
using v8::FunctionCallbackInfo;
using v8::FunctionTemplate;
using v8::Isolate;
using v8::Local;
using v8::Number;
using v8::Object;
using v8::Persistent;
using v8::String;
using v8::Value;

Persistent<Function> MyObject::constructor;

MyObject::MyObject(double value) : value_(value) {
}

MyObject::~MyObject() {
}

void MyObject::Init(Local<Object> exports) {
  Isolate* isolate = exports->GetIsolate();

  // Prepare constructor template
  Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
  tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
  tpl->InstanceTemplate()->SetInternalFieldCount(1);

  // Prototype
  NODE_SET_PROTOTYPE_METHOD(tpl, "plusOne", PlusOne);

  constructor.Reset(isolate, tpl->GetFunction());
  exports->Set(String::NewFromUtf8(isolate, "MyObject"),
               tpl->GetFunction());
}

void MyObject::New(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  if (args.IsConstructCall()) {
    // Invoked as constructor: `new MyObject(...)`
    double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
    MyObject* obj = new MyObject(value);
    obj->Wrap(args.This());
    args.GetReturnValue().Set(args.This());
  } else {
    // Invoked as plain function `MyObject(...)`, turn into construct call.
    const int argc = 1;
    Local<Value> argv[argc] = { args[0] };
    Local<Function> cons = Local<Function>::New(isolate, constructor);
    args.GetReturnValue().Set(cons->NewInstance(argc, argv));
  }
}

void MyObject::PlusOne(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  MyObject* obj = ObjectWrap::Unwrap<MyObject>(args.Holder());
  obj->value_ += 1;

  args.GetReturnValue().Set(Number::New(isolate, obj->value_));
}

}  // namespace demo
```

要构建这个例子，`myobject.cc` 文件必须被添加到 `binding.gyp`：

```c++
{
    "targets": [
        {
            "target_name": "addon",
            "sources": [
                "addon.cc",
                "myobject.cc"
            ]
        }
   ]
}
```

测试：

```javascript
// test.js
const addon = require('./build/Release/addon');

var obj = new addon.MyObject(10);
console.log(obj.plusOne()); // 11
console.log(obj.plusOne()); // 12
console.log(obj.plusOne()); // 13
```


<div id="factory_of_wrapped_objects" class="anchor"></div>
## 包装对象的工厂

另外，它可以使用一个工厂模式，以避免显式地使用 JavaScript 的 `new` 操作来创建对象的实例：

```javascript
var obj = addon.createObject();
// instead of:
// var obj = new addon.Object();
```

首先，在 `addon.cc` 中实现 `createObject()` 方法：

```c++
// addon.cc
#include <node.h>
#include "myobject.h"

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void CreateObject(const FunctionCallbackInfo<Value>& args) {
  MyObject::NewInstance(args);
}

void InitAll(Local<Object> exports, Local<Object> module) {
  MyObject::Init(exports->GetIsolate());

  NODE_SET_METHOD(module, "exports", CreateObject);
}

NODE_MODULE(addon, InitAll)

}  // namespace demo
```

在 `myobject.h` 中，添加静态方法 `NewInstance()` 来处理实例化对象。这种方法需要在 JavaScript 中使用 `new`：

```c++
// myobject.h
#ifndef MYOBJECT_H
#define MYOBJECT_H

#include <node.h>
#include <node_object_wrap.h>

namespace demo {

class MyObject : public node::ObjectWrap {
 public:
  static void Init(v8::Isolate* isolate);
  static void NewInstance(const v8::FunctionCallbackInfo<v8::Value>& args);

 private:
  explicit MyObject(double value = 0);
  ~MyObject();

  static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
  static void PlusOne(const v8::FunctionCallbackInfo<v8::Value>& args);
  static v8::Persistent<v8::Function> constructor;
  double value_;
};

}  // namespace demo

#endif
```

在 `myobject.cc` 中的实现类似与之前的例子：

```c++
// myobject.cc
#include <node.h>
#include "myobject.h"

namespace demo {

using v8::Function;
using v8::FunctionCallbackInfo;
using v8::FunctionTemplate;
using v8::Isolate;
using v8::Local;
using v8::Number;
using v8::Object;
using v8::Persistent;
using v8::String;
using v8::Value;

Persistent<Function> MyObject::constructor;

MyObject::MyObject(double value) : value_(value) {
}

MyObject::~MyObject() {
}

void MyObject::Init(Isolate* isolate) {
  // Prepare constructor template
  Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
  tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
  tpl->InstanceTemplate()->SetInternalFieldCount(1);

  // Prototype
  NODE_SET_PROTOTYPE_METHOD(tpl, "plusOne", PlusOne);

  constructor.Reset(isolate, tpl->GetFunction());
}

void MyObject::New(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  if (args.IsConstructCall()) {
    // Invoked as constructor: `new MyObject(...)`
    double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
    MyObject* obj = new MyObject(value);
    obj->Wrap(args.This());
    args.GetReturnValue().Set(args.This());
  } else {
    // Invoked as plain function `MyObject(...)`, turn into construct call.
    const int argc = 1;
    Local<Value> argv[argc] = { args[0] };
    Local<Function> cons = Local<Function>::New(isolate, constructor);
    args.GetReturnValue().Set(cons->NewInstance(argc, argv));
  }
}

void MyObject::NewInstance(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  const unsigned argc = 1;
  Local<Value> argv[argc] = { args[0] };
  Local<Function> cons = Local<Function>::New(isolate, constructor);
  Local<Object> instance = cons->NewInstance(argc, argv);

  args.GetReturnValue().Set(instance);
}

void MyObject::PlusOne(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  MyObject* obj = ObjectWrap::Unwrap<MyObject>(args.Holder());
  obj->value_ += 1;

  args.GetReturnValue().Set(Number::New(isolate, obj->value_));
}

}  // namespace demo
```

再来一次，建立这个例子，`myobject.cc` 文件必须被添加到 `binding.gyp`：

```javascript
{
    "targets": [
        {
            "target_name": "addon",
            "sources": [
                "addon.cc",
                "myobject.cc"
            ]
        }
   ]
}
```

测试它：

```javascript
// test.js
const createObject = require('./build/Release/addon');

var obj = createObject(10);
console.log(obj.plusOne()); // 11
console.log(obj.plusOne()); // 12
console.log(obj.plusOne()); // 13

var obj2 = createObject(20);
console.log(obj2.plusOne()); // 21
console.log(obj2.plusOne()); // 22
console.log(obj2.plusOne()); // 23
```


<div id="passing_wrapped_objects_around" class="anchor"></div>
## 传递包装的对象

除了包装和返回的 C++ 对象，也有可能是通过 Node.js 的辅助函数 `node::ObjectWrap::Unwrap` 展开传过来的包装对象。下面的例子显示了一个 `add()` 函数可以采用两个 `MyObject` 对象作为输入参数：

```c++
// addon.cc
#include <node.h>
#include <node_object_wrap.h>
#include "myobject.h"

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Number;
using v8::Object;
using v8::String;
using v8::Value;

void CreateObject(const FunctionCallbackInfo<Value>& args) {
  MyObject::NewInstance(args);
}

void Add(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  MyObject* obj1 = node::ObjectWrap::Unwrap<MyObject>(
      args[0]->ToObject());
  MyObject* obj2 = node::ObjectWrap::Unwrap<MyObject>(
      args[1]->ToObject());

  double sum = obj1->value() + obj2->value();
  args.GetReturnValue().Set(Number::New(isolate, sum));
}

void InitAll(Local<Object> exports) {
  MyObject::Init(exports->GetIsolate());

  NODE_SET_METHOD(exports, "createObject", CreateObject);
  NODE_SET_METHOD(exports, "add", Add);
}

NODE_MODULE(addon, InitAll)

}  // namespace demo
```

在 `myobject.h` 中，添加了一个公共方法以允许在展开对象后访问私有值。

```c++
// myobject.h
#ifndef MYOBJECT_H
#define MYOBJECT_H

#include <node.h>
#include <node_object_wrap.h>

namespace demo {

class MyObject : public node::ObjectWrap {
 public:
  static void Init(v8::Isolate* isolate);
  static void NewInstance(const v8::FunctionCallbackInfo<v8::Value>& args);
  inline double value() const { return value_; }

 private:
  explicit MyObject(double value = 0);
  ~MyObject();

  static void New(const v8::FunctionCallbackInfo<v8::Value>& args);
  static v8::Persistent<v8::Function> constructor;
  double value_;
};

}  // namespace demo

#endif
```

在 `myobject.cc` 中的实现类似与之前的例子：

```c++
// myobject.cc
#include <node.h>
#include "myobject.h"

namespace demo {

using v8::Function;
using v8::FunctionCallbackInfo;
using v8::FunctionTemplate;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::Persistent;
using v8::String;
using v8::Value;

Persistent<Function> MyObject::constructor;

MyObject::MyObject(double value) : value_(value) {
}

MyObject::~MyObject() {
}

void MyObject::Init(Isolate* isolate) {
  // Prepare constructor template
  Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
  tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
  tpl->InstanceTemplate()->SetInternalFieldCount(1);

  constructor.Reset(isolate, tpl->GetFunction());
}

void MyObject::New(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  if (args.IsConstructCall()) {
    // Invoked as constructor: `new MyObject(...)`
    double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
    MyObject* obj = new MyObject(value);
    obj->Wrap(args.This());
    args.GetReturnValue().Set(args.This());
  } else {
    // Invoked as plain function `MyObject(...)`, turn into construct call.
    const int argc = 1;
    Local<Value> argv[argc] = { args[0] };
    Local<Function> cons = Local<Function>::New(isolate, constructor);
    args.GetReturnValue().Set(cons->NewInstance(argc, argv));
  }
}

void MyObject::NewInstance(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();

  const unsigned argc = 1;
  Local<Value> argv[argc] = { args[0] };
  Local<Function> cons = Local<Function>::New(isolate, constructor);
  Local<Object> instance = cons->NewInstance(argc, argv);

  args.GetReturnValue().Set(instance);
}

}  // namespace demo
```

测试：

```javascript
// test.js
const addon = require('./build/Release/addon');

var obj1 = addon.createObject(10);
var obj2 = addon.createObject(20);
var result = addon.add(obj1, obj2);

console.log(result); // 30
```


<div id="AtExit_hooks" class="anchor"></div>
## AtExit 挂钩

“AtExit” 挂钩是一个函数，它是在 Node.js 事件循环结束后，但在 JavaScript 虚拟机被终止或 Node.js 关闭前调用。“AtExit” 挂钩使用 `node::AtExit` API 注册。


<div id="AtExit" class="anchor"></div>
### void AtExit(callback, args)

* `callback`：`void (*)(void*)` - 一个退出时调用的函数的指针。

* `args`：`void*` - 一个退出时传递给回调的指针。

注册在事件循环结束后并在虚拟机被终止前运行的退出挂钩。

AtExit 有两个参数：指向一个在退出运行的回调函数，和要传递给该回调的无类型的上下文数据的指针。

回调按照后进先出的顺序执行。

下面的 `addon.cc` 实现了 AtExit：

```c++
// addon.cc
#undef NDEBUG
#include <assert.h>
#include <stdlib.h>
#include <node.h>

namespace demo {

using node::AtExit;
using v8::HandleScope;
using v8::Isolate;
using v8::Local;
using v8::Object;

static char cookie[] = "yum yum";
static int at_exit_cb1_called = 0;
static int at_exit_cb2_called = 0;

static void at_exit_cb1(void* arg) {
  Isolate* isolate = static_cast<Isolate*>(arg);
  HandleScope scope(isolate);
  Local<Object> obj = Object::New(isolate);
  assert(!obj.IsEmpty()); // assert VM is still alive
  assert(obj->IsObject());
  at_exit_cb1_called++;
}

static void at_exit_cb2(void* arg) {
  assert(arg == static_cast<void*>(cookie));
  at_exit_cb2_called++;
}

static void sanity_check(void*) {
  assert(at_exit_cb1_called == 1);
  assert(at_exit_cb2_called == 2);
}

void init(Local<Object> exports) {
  AtExit(sanity_check);
  AtExit(at_exit_cb2, cookie);
  AtExit(at_exit_cb2, cookie);
  AtExit(at_exit_cb1, exports->GetIsolate());
}

NODE_MODULE(addon, init);

}  // namespace demo
```

在 JavaScript 中运行测试：

```javascript
// test.js
const addon = require('./build/Release/addon');
```