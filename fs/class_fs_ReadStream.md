# fs.ReadStream类

* ['open' 事件](#event_open)
* [readStream.path](#path)

--------------------------------------------------


`ReadStream` 是一个 [可读流](../stream/api_for_stream_consumers.md#class_Readable)。

<div id="event_open" class="anchor"></div>
## 'open' 事件

* `fd` {Number} 给 ReadStream 使用整数文件描述符。

在打开 ReadStream 文件时触发。


<div id="path" class="anchor"></div>
## readStream.path

流读取的文件路径。