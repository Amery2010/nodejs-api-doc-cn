# 压缩解压类

* [zlib.Zlib 类](#Zlib)
  - [zlib.flush([kind], callback)](#flush)
  - [zlib.params(level, strategy, callback)](#params)
  - [zlib.reset()](#reset)
* [zlib.Gzip 类](#Gzip)
* [zlib.Gunzip 类](#Gunzip)
* [zlib.Unzip 类](#Unzip)
* [zlib.Deflate 类](#Deflate)
* [zlib.Inflate 类](#Inflate)
* [zlib.DeflateRaw 类](#DeflateRaw)
* [zlib.InflateRaw 类](#InflateRaw)

--------------------------------------------------


<div id="Zlib" class="anchor"></div>
## zlib.Zlib 类

这个类未被 `zlib` 模块导出，编入此文档是因为它是其它压缩器/解压缩器的基类。


<div id="flush" class="anchor"></div>
### zlib.flush([kind], callback)

`kind` 默认为 `zlib.Z_FULL_FLUSH`。

Flush 待处理的数据。请勿轻易调用此方法，过早的 flush 会影响压缩算法的有效性。

调用该方法只能 flush 从 zlib 获取的内部状态数据，并且不会执行任何流级别类型的 flushing。它的行为反而像一个正常的 `.write()` 回调，例如，当从流读出数据时，它被排着其他未处理的写入后面并且只会产生输出。


<div id="params" class="anchor"></div>
### zlib.params(level, strategy, callback)

动态更新压缩级别和压缩策略。仅适用于 deflate 算法。


<div id="reset" class="anchor"></div>
### zlib.reset()

将压缩器/解压缩器重置为默认出厂值。仅对 inflate 和 deflate 算法有效。


<div id="Gzip" class="anchor"></div>
## zlib.Gzip 类

使用 Gzip 压缩数据。


<div id="Gunzip" class="anchor"></div>
## zlib.Gunzip 类

解压缩一个 Gunzip 流。


<div id="Unzip" class="anchor"></div>
## zlib.Unzip 类

通过自动检测报头来解压缩一个以 Gzip 或 Deflate 压缩的流。


<div id="Deflate" class="anchor"></div>
## zlib.Deflate 类

使用 Deflate 压缩数据。


<div id="Inflate" class="anchor"></div>
## zlib.Inflate 类

解压缩一个 Inflate 流。


<div id="DeflateRaw" class="anchor"></div>
## zlib.DeflateRaw 类

使用 DeflateRaw 压缩数据，并且不追加 Zlib 头。


<div id="InflateRaw" class="anchor"></div>
## zlib.InflateRaw 类

解压缩一个 InflateRaw 流。