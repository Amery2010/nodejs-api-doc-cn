# 流(Stream)

> 稳定度：2 - 稳定

流是一个被 Node.js 中很多对象所实现的抽象接口。比如[对一个 HTTP 服务器的请求](../http/class_http_IncomingMessage.md#)是一个流，[process.stdout](../process/process.md#stdout) 也是一个流。流是可读、可写或兼具两者的。所有流都是 [EventEmitter](../events/class_EventEmitter.md#) 的实例。

您可以通过 `require('stream')` 加载 `Stream` 基类，这些基类提供了 [Readable](./api_for_stream_consumers.md#class_Readable) 流、[Writable](./api_for_stream_consumers.md#class_Writable) 流、[Duplex](./api_for_stream_consumers.md#class_Duplex) 流和 [Transform](./api_for_stream_consumers.md#class_Transform) 流。

本文档分为三个章节：

1. 第一章节介绍了，你在你的程序中使用流时，需要了解的那部分 API 。

2. 第二章节介绍了，当你自己实现一个流时，需要用到的那部分 API ，这些 API 是为了方便你这么做而设计的。

3. 第三章节深入讲解了流的工作方式，包括一些内部机制和函数，除非你明确知道你在做什么，否则尽量不要改动它们。