# 关于本文档

无论从参考还是对概念理解的角度来看，本文档的目的都是**全面解释Node.js的API**。文档的每个部分都会介绍一个内置的模块或更高层次的概念。

在某些情况下，属性类型、方法参数以及事件处理过程(handler)参数会被列在主标题下的列表中。

<del>每一个`.html`文件都对应一份内容相同的结构化`.json`文档。这个特性现在还是实验性质的，希望能够为一些需要对文档进行操作的IDE或者其他工具提供帮助。</del>

<del>每个`.html`和`.json`文件都是基于源码的`doc/api/`目录下的`.markdown`文件生成的。本文档使用`tools/doc/generate.js`这个程序来生产的。`HTML`模板文件为`doc/template.html`。</del>

（目前该中文版文档采用未采用原始工具生成，如需查阅json格式请转至[官方文档](https://nodejs.org/dist/latest-v5.x/docs/api/)查阅，[译者](https://github.com/Amery2010)注）

如果你在阅读过程中发现文档中的错误，请[提交问题(issue)](https://github.com/nodejs/node/issues/new)或查阅[文档贡献指南](https://github.com/nodejs/node/blob/master/CONTRIBUTING.md)中关于如何提交问题的操作说明。

（如果你在阅读过程发现文档翻译上的错误，请[提交问题(issue)](https://github.com/Amery2010/nodejs-api-book/issues/new)）