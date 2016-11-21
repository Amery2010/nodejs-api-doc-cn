# 选项(Options)

* [-v, --version](#v-version)
* [-h, --help](#h-help)
* [-e, --eval "script"](#e-eval-script)
* [-p, --print "script"](#p-print-script)
* [-c, --check](#c-check)
* [-i, --interactive](#i-interactive)
* [-r, --require module](#r-require-module)
* [--no-deprecation](#nodeprecation)
* [--trace-deprecation](#tracedeprecation)
* [--throw-deprecation](#throwdeprecation)
* [--trace-sync-io](#tracesyncio)
* [--zero-fill-buffers](#zerofillbuffers)
* [--track-heap-objects](#trackheapobjects)
* [--prof-process](#profprocess)
* [--v8-options](#v8options)
* [--tls-cipher-list=list](#tlscipherlistlist)
* [--enable-fips](#enablefips)
* [--force-fips](#forcefips)
* [--icu-data-dir=file](#icudatadirfile)

----------------------------------------


## -v, --version

打印 Node.js 的版本号。


## -h, --help

打印 Node.js 的命令行选项。此选项的输出不如本文档详细。


## -e, --eval "script"

将以下参数作为 JavaScript 进行评估。在 REPL 中预定义的模块也可以在 `script` 中使用。


## -p, --print "script"

与 `-e` 相同，但会打印结果。


## -c, --check

在不执行的情况下，对脚本进行语法检查。


## -i, --interactive

打开 REPL，即便 stdin 看起来不像终端。


## -r, --require module

在启动时预加载指定的模块。

遵循 `require()` 的模块解析规则。`module` 可以是文件的路径，也可以是 Node.js 的模块名称。


## --no-deprecation

静默废弃的警告。


## --trace-deprecation

打印废弃的堆栈跟踪。


## --throw-deprecation

抛出废弃的错误。


## --trace-sync-io

每当在事件循环的第一帧之后检测到同步 I/O 时，打印堆栈跟踪。


## --zero-fill-buffers

自动填充所有新分配的 [Buffer](../buffer/class_Buffer.md#) 和 [SlowBuffer](../buffer/class_SlowBuffer.md#) 实例。


## --track-heap-objects

为堆快照分配的堆栈对象。


## --prof-process

使用 v8 选项 `--prof` 处理 v8 分析器生成的输出。


## --v8-options

打印 v8 命令行选项。


## --tls-cipher-list=list

指定备用的默认 TLS 加密列表。（需要使用支持的加密构建 Node.js 。（默认））


## --enable-fips

启动时启用符合 FIPS 标准的加密。（需要使用 `./configure --openssl-fips` 构建 Node.js。）


## --icu-data-dir=file

指定 ICU 数据的加载路径。（覆盖 NODE_ICU_DATA）