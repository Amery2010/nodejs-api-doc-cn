# 系统错误

* [System Error类](#class_System_Error)
  - [error.errno](#errno)
  - [error.code](#code)
  - [error.syscall](#syscall)
* [通用的系统错误](#common_cystem_errors)

--------------------------------------------------


系统错误通常在程序运行环境中引发的异常。当应用程序违反了操作系统的限制时通常会引发操作错误，例如试图读取一个不存在的文件或用户没有足够的权限。

系统错误通常是在系统调用（syscall）的级别产生：一组错误代码的详尽清单和它们的含义在线上或大多数的 Unix 上运行 `man 2 intro` 或 `man 3 errno` 是可行的。

在 Node.js 中，系统错误表现为已传参的 `Error` 对象的附加属性。


<div id="class_System_Error" class="anchor"></div>
## System Error类


<div id="errno" class="anchor"></div>
#### error.errno

返回表示错误代码的字符串，经常是 `E` 后面跟着一串大写字母，并有可能是 `man 2 intro` 中的引用。

`error.code` 和 `error.errno` 属性是彼此的别名并返回相同的值。


<div id="code" class="anchor"></div>
#### error.code

同 [error.errno](#errno)。


<div id="syscall" class="anchor"></div>
#### error.syscall

返回描述失败的系统调用（[syscall](http://man7.org/linux/man-pages/man2/syscall.2.html)）的字符串。


<div id="common_cystem_errors" class="anchor"></div>
## 通用的系统错误

这个列表并**不详尽**，但列举了写一个 Node.js 程序时可能遇到的许多常见系统错误。一个详尽的清单可以在[这里](http://man7.org/linux/man-pages/man3/errno.3.html)找到。

* `EACCES`（没有权限）：试图在一个文件权限不允许的文件夹中访问一个文件。

* `EADDRINUSE`（地址已被使用）：试图给一个服务器（[net](../net/)、[http](../http/) 或 [https](../https/)）绑定本地地址因另外一个在本地系统中服务器已经占据了这个地址所导致的失败。

* `ECONNREFUSED`（连接被拒绝）：目标机器积极拒绝导致的无法连接。这通常是试图连接到国外主机上不活动的服务后的结果。

* `ECONNRESET`（连接被对方重置）：一个连接被对方强行关闭。这通常是因超时或自动重启导致的远程通道（socket）丢失连接的结果。在 [http](../http/) 和 [net](../net/) 模块中经常会碰到。

* `EEXIST`（文件已存在）：一个要求目标不存在的文件操作的目标是一个已存在的文件。

* `EISDIR`（是一个目录）：一个对文件的操作，但给定的路径是一个目录。

* `EMFILE`（在系统中打开了过多的文件）：