# fs.WriteStream类

* ['open' 事件](#event_open)
* [writeStream.path](#path)
* [writeStream.bytesWritten](#bytesWritten)

--------------------------------------------------


`WriteStream` 是一个 [可写流](../stream/api_for_stream_consumers.md#class_Writable)。

<div id="event_open" class="anchor"></div>
## 'open' 事件

* `fd` {Number} 给 WriteStream 使用整数文件描述符。

在打开 WriteStream 文件时触发。


<div id="path" class="anchor"></div>
## writeStream.path

流写入的文件路径。


<div id="bytesWritten" class="anchor"></div>
## writeStream.bytesWritten

迄今为止写入的字节数。不包括仍在排队等待写入的数据。