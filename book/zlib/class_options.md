# 类参数

每个类需要一个 options 对象。所有选项都是可选的。

请注意有些选项仅对压缩有效，并会被解压缩类所忽略。

* flush（默认：`zlib.Z_NO_FLUSH`）

* finishFlush（默认：`zlib.Z_FINISH`）

* chunkSize（默认：16*1024）

* windowBits

* level（仅用于压缩）

* memLevel（仅用于压缩）

* strategy（仅用于压缩）

* dictionary（仅用于 deflate/inflate，缺省为空目录）

`deflateInit2` 和 `inflateInit2` 的描述可以在 [http://zlib.net/manual.html#Advanced](http://zlib.net/manual.html#Advanced) 上查阅到更多内容。