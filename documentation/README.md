# 关于本文档

* [稳定度](./stability_index.md)
* [JSON 格式输出](./json_output.md)
* [系统调用和手册页](./syscalls_and_man_pages.md)

-------------------------------------------------------

无论从参考还是从概念的角度来看，本文档的目的都是**全面解释 Node.js API**。文档的每个部分都会介绍一个内置的模块或更高层次的概念。

在某些情况下，属性类型，方法参数和提供给事件处理程序的参数在主题标题下的列表中有详细说明。

每个 `.html` 文档都有一个相应的 `.json` 文档，以结构化的方式呈现相同的信息。这个特性是实验性的，希望能够为一些需要对文档进行程序化操作的 IDE 或者其他工具提供帮助。

每个 `.html` 和 `.json` 文件都是基于源代码 `doc/api/` 目录下的 `.md` 文件生成的。本文档使用 `tools/doc/generate.js` 这个程序生成。`HTML` 模板位于 `doc/template.html`。

*目前本文档是基于 [Gitbook](https://www.gitbook.com) 生成的，如需查阅 `json` 格式的文档请转至[官方文档](https://nodejs.org/dist/latest-v5.x/docs/api/)查阅，[译者](https://github.com/Amery2010)注*

如果你在阅读过程中发现文档中的错误，请[提交问题(issue)](https://github.com/nodejs/node/issues/new)或查阅[文档贡献指南](https://github.com/nodejs/node/blob/master/CONTRIBUTING.md)中关于如何提交问题的操作说明。

*如果你在阅读过程发现翻译上的错误，请[提交问题(issue)](https://github.com/Amery2010/nodejs-api-doc-cn/issues/new)。*