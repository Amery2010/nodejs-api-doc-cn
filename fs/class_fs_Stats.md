# fs.Stats类

* [状态时间值](#stat_time_values)

--------------------------------------------------


从 [fs.stat()](#stat)、[fs.lstat()](#lstat) 和 [fs.fstat()](#fstat) 及其同步版本返回的对象都属于这种类型。

* `stats.isFile()`

* `stats.isDirectory()`

* `stats.isBlockDevice()`

* `stats.isCharacterDevice()`

* `stats.isSymbolicLink()`（只在 [fs.lstat()](#lstat) 中有效）

* `stats.isFIFO()`

* `stats.isSocket()`

对于一个普通文件，[util.inspect(stats)](../util/util.md#inspect) 将返回非常类似这样的字符串：

``` javascript
{
    dev: 2114,
    ino: 48064969,
    mode: 33188,
    nlink: 1,
    uid: 85,
    gid: 100,
    rdev: 0,
    size: 527,
    blksize: 4096,
    blocks: 8,
    atime: Mon, 10 Oct 2011 23:24:11 GMT,
    mtime: Mon, 10 Oct 2011 23:24:11 GMT,
    ctime: Mon, 10 Oct 2011 23:24:11 GMT,
    birthtime: Mon, 10 Oct 2011 23:24:11 GMT
}
```

请注意 `atime`、`mtime`、`birthtime` 和 `ctime` 是 [Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date) 对象的实例，比较这些对象的值，你应该使用合适的方法。对于大多数一般用途 [getTime()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime) 会返回自 *1 January 1970 00:00:00 UTC* 已过的毫秒数并且该整数足以拿来做任何比较，然而也有可用于显示模糊信息的其他方法。更多的细节可以在 [MDN JavaScript的参考页](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date) 中找到。


<div id="stat_time_values" class="anchor"></div>
## 状态时间值

在状态对象中的时间有以下语义：

* `atime` “访问时间” - 文件数据最后被访问的时间。会被 `mknod(2)`、`utimes(2)` 和 `read(2)` 系统调用更改。

* `mtime` “修改时间” - 文件数据最后被修改的时间。会被 `mknod(2)`、`utimes(2)` 和 `write(2)` 系统调用更改。

* `ctime` “更改时间” - 文件状态上次更改的时间（索引节点数据修改）。会被 `chmod(2)`、`chown(2)`、`link(2)`、`mknod(2)`、`rename(2)`、`unlink(2)`、`utimes(2)`、`read(2)` 和 `write(2)`系统调用更改。

* `birthtime` “出生时间” - 文件创建的时间。在创建文件时设定一次。在文件系统中出生日期不可用，这个字段可能代替保存 `ctime` 或 `1970-01-01T00:00Z`（如，Unix 的纪元时间戳 `0`）两者任一。注意，此值也许比在此情况下的 `atime` 或 `mtime` 更加有用。在 Darwin 和其它的 FreeBSD 衍生系统中，如果时间被明确设置到了一个比目前出生时间较早的值，也会设置使用 `utimes(2)` 系统调用。

在 Node.js v0.12 之前的版本中，在 Windows 系统中，`ctime` 保存 `birthtime`。请注意在 v0.12 中，`ctime` 不是“创建时间”，并且在 Unix 系统中，它从来都不是。