# 系统调用和手册页

系统调用定义了用户程序和底层操作系统之间的接口，例如 [open(2)](http://man7.org/linux/man-pages/man2/open.2.html) 和 [read(2)](http://man7.org/linux/man-pages/man2/read.2.html) 。Node函数只是简单的包装了一个系统调用，比如 `fs.open()` 中的文档提示。该文档链接到相应的手册页（以下简称手册页），其中描述了该系统调用的工作方式。

警告：一些系统调用，像 [lchown(2)](http://man7.org/linux/man-pages/man2/lchown.2.html) ，都是BSD系统特定的。这就意味着 `fs.chown()` 只适用于Mac OS X和其他的BSD派生的系统， 在Linux上是不可用。

Windows 环境下的大多数系统回调和 Unix 环境下的一样，不过有一些可能与 Linux 和 MAC OS X 不同。以一种微妙的关系为例，Windows 环境下有时不可能找到某些 Unix 系统回调的替代方案，详见 [Node issue 4760](https://github.com/nodejs/node/issues/4760) 。