# 系统错误

* [系统错误类](#系统错误类)
  - [error.errno](#errorerrno)
  - [error.code](#errorcode)
  - [error.syscall](#errorsyscall)
* [通用的系统错误](#通用的系统错误)

--------------------------------------------------


当程序在运行时环境中发生异常时会产生系统错误。通常，这些是当应用违反操作系统约束（例如尝试读取不存在的文件或当用户没有足够的权限）时发生的操作错误。

系统错误通常是在系统调用（syscall）级别产生：通过在大多数 Unix 上运行 `man 2 intro` 或 `man 3 errno` 或[在线](http://man7.org/linux/man-pages/man3/errno.3.html)查找，可以获取错误代码及其含义的详尽列表。

在 Node.js 中，系统错误表现为添加额外属性的增强型 `Error` 对象。


## 系统错误类


### error.errno

返回表示**否定**错误码相应的数字，可能引用自 `man 2 intro`。例如，`ENOENT` 错误的 `errno` 的值为 `-2`，因为 `ENOENT` 的错误码为 `2`。


### error.code

返回一个表示错误码的字符串，它总是 `E` 后面跟着一串大写字母，并可能引用自 `man 2 intro`。


### error.syscall

返回描述失败的系统调用（[syscall](http://man7.org/linux/man-pages/man2/syscall.2.html)）的字符串。


## 通用的系统错误

这个列表并**不详尽**，但列举了开发 Node.js 程序时可能遇到的许多常见的系统错误。在[这里](http://man7.org/linux/man-pages/man3/errno.3.html)可以找到一个详尽的列表。

* `EACCES`（没有权限）：试图在一个文件权限不允许的文件夹中访问一个文件。

* `EADDRINUSE`（地址已被使用）：试图将一个服务器（[net](../net/)、[http](../http/) 或 [https](../https/)）绑定本地地址失败，原因是本地系统上的另一个服务已占用了该地址。

* `ECONNREFUSED`（连接被拒绝）：目标机器积极拒绝导致的无法连接。这通常是试图连接到国外主机上不活动的服务后的结果。

* `ECONNRESET`（连接被对方重置）：一个连接被对方强行关闭。这通常是因超时或自动重启导致的远程套接字（socket）丢失的结果。在 [http](../http/) 和 [net](../net/) 模块中经常会碰到。

* `EEXIST`（文件已存在）：一个要求目标不存在的文件操作的目标是一个已存在的文件。

* `EISDIR`（是一个目录）：一个对文件的操作，但给定的路径是一个目录。

* `EMFILE`（在系统中打开了过多的文件）：已达到系统中[文件描述符](https://en.wikipedia.org/wiki/File_descriptor)允许的最大数量，并且另外一个描述符的请求在至少关闭其中一个之前不能被满足。这在一次并行打开多个文件时会遇到，尤其是在那些在进程中限制了一个较低的文件描述符数量的操作系统上（在 particular 和 OS X 中）。为了破除这个限制，请在与运行 Node.js 进程的同一 `shell` 中运行 `ulimit -n 2048` 。

* `ENOENT`（无此文件或目录）：通常是由[文件操作](../fs/)引起的，这表明指定的路径组合不存在——在给定的路径上无法找到任何实体（文件或目录）。

* `ENOTDIR`（不是一个目录）：给定的路径组合存在，但不是所期望的目录。通常是由 [fs.readdir](../fs/fs.md#readdir) 引起的。

* `ENOTEMPTY`（目录非空）：一个需要空目录操作的目标目录是一个实体。通常是由 [fs.unlink](../fs/fs.md#unlink) 引起的。

* `EPERM`（操作不被允许）：试图执行需要提升权限的操作。

* `EPIPE`（管道损坏）：没有进程读取数据时写入 `pipe`、`socket` 或 `FIFO`。在 [net](../net/) 和[http](../http/) 层经常遇到，表明在远端的流（stream）准备写入前被关闭。

* `ETIMEDOUT`（操作超时）：由于连接方在一段时间后并没有做出合适的响应导致的连接或发送的请求失败。在 [http](../http/) 或 [net](../net/) 中经常遇到——往往标志着 `socket.end()` 没有被合适的调用。