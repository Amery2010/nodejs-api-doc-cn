# 常量

所有定义在 zlib.h 上的常量也同样定义在 `require('zlib')`。在正常的操作过程中，你几乎不会用到这些。编入文档只是为了让你对它们的存在不会感到意外。该章节几乎完全来自 [zlib 的文档](http://zlib.net/manual.html#Constants)。详见 [http://zlib.net/manual.html#Constants](http://zlib.net/manual.html#Constants)。

允许的 flush 值。

* `zlib.Z_NO_FLUSH`

* `zlib.Z_PARTIAL_FLUSH`

* `zlib.Z_SYNC_FLUSH`

* `zlib.Z_FULL_FLUSH`

* `zlib.Z_FINISH`

* `zlib.Z_BLOCK`

* `zlib.Z_TREES`

压缩/解压缩函数的返回值。负数代表错误，正数代表特殊但正常的事件。

* `zlib.Z_OK`

* `zlib.Z_STREAM_END`

* `zlib.Z_NEED_DICT`

* `zlib.Z_ERRNO`

* `zlib.Z_STREAM_ERROR`

* `zlib.Z_DATA_ERROR`

* `zlib.Z_MEM_ERROR`

* `zlib.Z_BUF_ERROR`

* `zlib.Z_VERSION_ERROR`

压缩级别。

* `zlib.Z_NO_COMPRESSION`

* `zlib.Z_BEST_SPEED`

* `zlib.Z_BEST_COMPRESSION`

* `zlib.Z_DEFAULT_COMPRESSION`

压缩策略。

* `zlib.Z_FILTERED`

* `zlib.Z_HUFFMAN_ONLY`

* `zlib.Z_RLE`

* `zlib.Z_FIXED`

* `zlib.Z_DEFAULT_STRATEGY`

data_type 字段的可能值。

* `zlib.Z_BINARY`

* `zlib.Z_TEXT`

* `zlib.Z_ASCII`

* `zlib.Z_UNKNOWN`

deflate 压缩方法（该版本仅支持一种）。

* `zlib.Z_DEFLATED`

初始化 zalloc / zfree / opaque。

* `zlib.Z_NULL`