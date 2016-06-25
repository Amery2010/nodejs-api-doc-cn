# 方法和属性

* [zlib.createGzip([options])](#createGzip)
* [zlib.createGunzip([options])](#createGunzip)
* [zlib.createUnzip([options])](#createUnzip)
* [zlib.createDeflate([options])](#createDeflate)
* [zlib.createInflate([options])](#createInflate)
* [zlib.createDeflateRaw([options])](#createDeflateRaw)
* [zlib.createInflateRaw([options])](#createInflateRaw)

### 便捷方法

* [zlib.gzip(buf[, options], callback)](#gzip)
* [zlib.gzipSync(buf[, options])](#gzipSync)
* [zlib.gunzip(buf[, options], callback)](#gunzip)
* [zlib.gunzipSync(buf[, options])](#gunzipSync)
* [zlib.unzip(buf[, options], callback)](#unzip)
* [zlib.unzipSync(buf[, options])](#unzipSync)
* [zlib.deflate(buf[, options], callback)](#deflate)
* [zlib.deflateSync(buf[, options])](#deflateSync)
* [zlib.inflate(buf[, options], callback)](#inflate)
* [zlib.inflateSync(buf[, options])](#inflateSync)
* [zlib.deflateRaw(buf[, options], callback)](#deflateRaw)
* [zlib.deflateRawSync(buf[, options])](#deflateRawSync)
* [zlib.inflateRaw(buf[, options], callback)](#inflateRaw)
* [zlib.inflateRawSync(buf[, options])](#inflateRawSync)

--------------------------------------------------


<div id="createGzip" class="anchor"></div>
## zlib.createGzip([options])

返回一个用 [options](./class_options.md#) 生成的新的 [Gzip](./zlib_class.md#Gzip) 对象。


<div id="createGunzip" class="anchor"></div>
## zlib.createGunzip([options])

返回一个用 [options](./class_options.md#) 生成的新的 [Gunzip](./zlib_class.md#Gunzip) 对象。


<div id="createUnzip" class="anchor"></div>
## zlib.createUnzip([options])

返回一个用 [options](./class_options.md#) 生成的新的 [Unzip](./zlib_class.md#Unzip) 对象。


<div id="createDeflate" class="anchor"></div>
## zlib.createDeflate([options])

返回一个用 [options](./class_options.md#) 生成的新的 [Deflate](./zlib_class.md#Deflate) 对象。


<div id="createInflate" class="anchor"></div>
## zlib.createInflate([options])

返回一个用 [options](./class_options.md#) 生成的新的 [Inflate](./zlib_class.md#Inflate) 对象。


<div id="createDeflateRaw" class="anchor"></div>
## zlib.createDeflateRaw([options])

返回一个用 [options](./class_options.md#) 生成的新的 [DeflateRaw](./zlib_class.md#DeflateRaw) 对象。


<div id="createInflateRaw" class="anchor"></div>
## zlib.createInflateRaw([options])

返回一个用 [options](./class_options.md#) 生成的新的 [InflateRaw](./zlib_class.md#InflateRaw) 对象。


<div id="gzip" class="anchor"></div>
### zlib.gzip(buf[, options], callback)
<div id="gzipSync" class="anchor"></div>
### zlib.gzipSync(buf[, options])

通过 Gzip 来压缩一个 Buffer 或字符串。


<div id="gunzip" class="anchor"></div>
### zlib.gunzip(buf[, options], callback)
<div id="gunzipSync" class="anchor"></div>
### zlib.gunzipSync(buf[, options])

通过 Gunzip 来解压缩一个 Buffer 或字符串。


<div id="unzip" class="anchor"></div>
### zlib.unzip(buf[, options], callback)
<div id="unzipSync" class="anchor"></div>
### zlib.unzipSync(buf[, options])

通过 Unzip 来解压缩一个 Buffer 或字符串。


<div id="deflate" class="anchor"></div>
### zlib.deflate(buf[, options], callback)
<div id="deflateSync" class="anchor"></div>
### zlib.deflateSync(buf[, options])

通过 Deflate 来压缩一个 Buffer 或字符串。


<div id="inflate" class="anchor"></div>
### zlib.inflate(buf[, options], callback)
<div id="inflateSync" class="anchor"></div>
### zlib.inflateSync(buf[, options])

通过 Inflate 来解压缩一个 Buffer 或字符串。


<div id="deflateRaw" class="anchor"></div>
### zlib.deflateRaw(buf[, options], callback)
<div id="deflateRawSync" class="anchor"></div>
### zlib.deflateRawSync(buf[, options])

通过 DeflateRaw 来压缩一个 Buffer 或字符串。


<div id="inflateRaw" class="anchor"></div>
### zlib.inflateRaw(buf[, options], callback)
<div id="inflateRawSync" class="anchor"></div>
### zlib.inflateRawSync(buf[, options])

通过 InflateRaw 来解压缩一个 Buffer 或字符串。