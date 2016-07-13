# 方法和属性

* [fs.readdir(path, callback)](#readdir)
* [fs.readdirSync(path)](#readdirSync)
* [fs.mkdir(path[, mode], callback)](#mkdir)
* [fs.mkdirSync(path[, mode])](#mkdirSync)
* [fs.rmdir(path, callback)](#rmdir)
* [fs.rmdirSync(path)](#rmdirSync)
* [fs.realpath(path[, cache], callback)](#realpath)
* [fs.realpathSync(path[, cache])](#realpathSync)
* [fs.link(srcpath, dstpath, callback)](#link)
* [fs.linkSync(srcpath, dstpath)](#linkSync)
* [fs.symlink(target, path[, type], callback)](#symlink)
* [fs.symlinkSync(target, path[, type])](#symlinkSync)
* [fs.readlink(path, callback)](#readlink)
* [fs.readlinkSync(path)](#readlinkSync)
* [fs.unlink(path, callback)](#unlink)
* [fs.unlinkSync(path)](#unlinkSync)
* [fs.lchmod(path, mode, callback)](#lchmod)
* [fs.lchmodSync(path, mode)](#lchmodSync)
* [fs.lchown(path, uid, gid, callback)](#lchown)
* [fs.lchownSync(path, uid, gid)](#lchownSync)
* [fs.lstat(path, callback)](#lstat)
* [fs.lstatSync(path)](#lstatSync)
* [fs.createReadStream(path[, options])](#createReadStream)
* [fs.read(fd, buffer, offset, length, position, callback)](#read)
* [fs.readSync(fd, buffer, offset, length, position)](#readSync)
* [fs.createWriteStream(path[, options])](#createWriteStream)
* [fs.write(fd, data[, position[, encoding]], callback)](#write_data)
* [fs.writeSync(fd, data[, position[, encoding]])](#write_data_sync)
* [fs.write(fd, buffer, offset, length[, position], callback)](#write_buffer)
* [fs.writeSync(fd, buffer, offset, length[, position])](#write_buffer_sync)
* [fs.truncate(path, len, callback)](#truncate)
* [fs.truncateSync(path, len)](#truncateSync)
* [fs.watch(filename[, options][, listener])](#watch)
* [fs.stat(path, callback)](#stat)
* [fs.statSync(path)](#statSync)
* [fs.chmod(path, mode, callback)](#chmod)
* [fs.chmodSync(path, mode)](#chmodSync)
* [fs.chown(path, uid, gid, callback)](#chown)
* [fs.chownSync(path, uid, gid)](#chownSync)
* [fs.utimes(path, atime, mtime, callback)](#utimes)
* [fs.utimesSync(path, atime, mtime)](#utimesSync)
* [fs.open(path, flags[, mode], callback)](#open)
* [fs.openSync(path, flags[, mode])](#openSync)
* [fs.close(fd, callback)](#close)
* [fs.closeSync(fd)](#closeSync)
* [fs.exists(path, callback)](#exists)
* [fs.existsSync(path)](#existsSync)
* [fs.rename(oldPath, newPath, callback)](#rename)
* [fs.renameSync(oldPath, newPath)](#renameSync)
* [fs.readFile(file[, options], callback)](#readFile)
* [fs.readFileSync(file[, options])](#readFileSync)
* [fs.writeFile(file, data[, options], callback)](#writeFile)
* [fs.writeFileSync(file, data[, options])](#writeFileSync)
* [fs.appendFile(file, data[, options], callback)](#appendFile)
* [fs.appendFileSync(file, data[, options])](#appendFileSync)
* [fs.ftruncate(fd, len, callback)](#ftruncate)
* [fs.ftruncateSync(fd, len)](#ftruncateSync)
* [fs.watchFile(filename[, options], listener)](#watchFile)
* [fs.unwatchFile(filename[, listener])](#unwatchFile)
* [fs.fstat(fd, callback)](#fstat)
* [fs.fstatSync(fd)](#fstatSync)
* [fs.access(path[, mode], callback)](#access)
* [fs.accessSync(path[, mode])](#accessSync)
* [fs.fchmod(fd, mode, callback)](#fchmod)
* [fs.fchmodSync(fd, mode)](#fchmodSync)
* [fs.fchown(fd, uid, gid, callback)](#fchown)
* [fs.fchownSync(fd, uid, gid)](#fchownSync)
* [fs.fdatasync(fd, callback)](#fdatasync)
* [fs.fdatasyncSync(fd)](#fdatasyncSync)
* [fs.futimes(fd, atime, mtime, callback)](#futimes)
* [fs.futimesSync(fd, atime, mtime)](#futimesSync)
* [fs.fsync(fd, callback)](#fsync)
* [fs.fsyncSync(fd)](#fsyncSync)

--------------------------------------------------


<div id="readdir" class="anchor"></div>
## fs.readdir(path, callback)

异步的 [readdir(3)](http://man7.org/linux/man-pages/man3/readdir.3.html)。读取目录的内容。回调带有两个参数 `(err, files)`，`files` 是该目录中不包括 `'.'` 和 `'..'` 的文件名的数组。


<div id="readdirSync" class="anchor"></div>
## fs.readdirSync(path)

同步的 [readdir(3)](http://man7.org/linux/man-pages/man3/readdir.3.html)。返回一个不包括 `'.'` 和 `'..'` 的文件名的数组。


<div id="mkdir" class="anchor"></div>
## fs.mkdir(path[, mode], callback)

异步的 [mkdir(2)](http://man7.org/linux/man-pages/man2/mkdir.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。`mode` 默认为 `0o777`。


<div id="mkdirSync" class="anchor"></div>
## fs.mkdirSync(path[, mode])

同步的 [mkdir(2)](http://man7.org/linux/man-pages/man2/mkdir.2.html)。返回 `undefined`。


<div id="rmdir" class="anchor"></div>
## fs.rmdir(path, callback)

异步的 [rmdir(2)](http://man7.org/linux/man-pages/man2/rmdir.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="rmdirSync" class="anchor"></div>
## fs.rmdirSync(path)

同步的 [rmdir(2)](http://man7.org/linux/man-pages/man2/rmdir.2.html)。返回 `undefined`。


<div id="realpath" class="anchor"></div>
## fs.realpath(path[, cache], callback)

异步的 [realpath(2)](http://man7.org/linux/man-pages/man2/realpath.2.html)。`callback` 有两个参数 `(err, resolvedPath)`。可以使用 `process.cwd` 解析相对路径。`cache` 是一个对象字面映射路径可用于强制一个特定路径的解析或避免 `fs.stat` 对已知的真实路径的额外调用。

例子：

``` javascript
var cache = {
    '/etc': '/private/etc'
};
fs.realpath('/etc/passwd', cache, (err, resolvedPath) => {
    if (err) throw err;
    console.log(resolvedPath);
});
```


<div id="realpathSync" class="anchor"></div>
## fs.realpathSync(path[, cache])

同步的 [realpath(2)](http://man7.org/linux/man-pages/man2/realpath.2.html)。返回解析的路径。`cache` 是一个对象字面映射路径可用于强制一个特定路径的解析或避免 `fs.stat` 对已知的真实路径的额外调用。


<div id="link" class="anchor"></div>
## fs.link(srcpath, dstpath, callback)

异步的 [link(2)](http://man7.org/linux/man-pages/man2/link.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="linkSync" class="anchor"></div>
## fs.linkSync(srcpath, dstpath)

同步的 [link(2)](http://man7.org/linux/man-pages/man2/link.2.html)。返回 `undefined`。


<div id="symlink" class="anchor"></div>
## fs.symlink(target, path[, type], callback)

异步的 [symlink(2)](http://man7.org/linux/man-pages/man2/symlink.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。`type` 参数可以设置为 `'dir'`、`'file'` 或 `'junction'`（默认为 `'file'`）并且仅在 Windows 上可用（在其他平台上忽略）。请注意，Windows 交接点需要的目标路径是绝对的。当使用 `'junction'` 时，`target` 参数会被自动归到绝对路径。

这里有以下例子：

``` javascript
fs.symlink('./foo', './new-port');
```

它创建了一个名为“new-port”，指向“foo”的一个符号链接。


<div id="symlinkSync" class="anchor"></div>
## fs.symlinkSync(target, path[, type])

同步的 [symlink(2)](http://man7.org/linux/man-pages/man2/symlink.2.html)。返回 `undefined`。


<div id="readlink" class="anchor"></div>
## fs.readlink(path, callback)

异步的 [readlink(2)](http://man7.org/linux/man-pages/man2/readlink.2.html)。回调参数有两个参数 `(err, linkString)`。


<div id="readlinkSync" class="anchor"></div>
## fs.readlinkSync(path)

同步的 [readlink(2)](http://man7.org/linux/man-pages/man2/readlink.2.html)。返回符号链接字符串值。


<div id="unlink" class="anchor"></div>
## fs.unlink(path, callback)

异步的 [unlink(2)](http://man7.org/linux/man-pages/man2/unlink.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="unlinkSync" class="anchor"></div>
## fs.unlinkSync(path)

同步的 [unlink(2)](http://man7.org/linux/man-pages/man2/unlink.2.html)。返回 `undefined`。


<div id="lchmod" class="anchor"></div>
## fs.lchmod(path, mode, callback)

异步的 [lchmod(2)](https://www.freebsd.org/cgi/man.cgi?query=lchmod&sektion=2)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。

仅在 Mac OS X 上有效。


<div id="lchmodSync" class="anchor"></div>
## fs.lchmodSync(path, mode)

同步的 [lchmod(2)](https://www.freebsd.org/cgi/man.cgi?query=lchmod&sektion=2)。返回 `undefined`。


<div id="lchown" class="anchor"></div>
## fs.lchown(path, uid, gid, callback)

异步的 [lchown(2)](http://man7.org/linux/man-pages/man2/lchown.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="lchownSync" class="anchor"></div>
## fs.lchownSync(path, uid, gid)

同步的 [lchown(2)](http://man7.org/linux/man-pages/man2/lchown.2.html)。返回 `undefined`。


<div id="lstat" class="anchor"></div>
## fs.lstat(path, callback)

异步的 [lstat(2)](http://man7.org/linux/man-pages/man2/lstat.2.html)。该回调函数带有两个参数 `(err, stats)`，其中 `stats` 是一个 `fs.Stats` 对象。`lstat()` 等同于 `stat()`，除非 `path` 是一个 symbolic 链接，那么自身就是该链接，它指向的并不是文件。


<div id="lstatSync" class="anchor"></div>
## fs.lstatSync(path)

同步的 [lstat(2)](http://man7.org/linux/man-pages/man2/lstat.2.html)。返回一个 `fs.Stats` 实例。


<div id="createReadStream" class="anchor"></div>
## fs.createReadStream(path[, options])

返回一个新的 [ReadStream](./class_fs_ReadStream.md#) 对象（详见 [可读流章节](./stream/api_for_stream_consumers.md#class_Readable)）。

要知道，不同于在一个可读流上设置的 `highWaterMark` 默认值（16 kb），此方法在相同参数下返回的流具有 64 kb 的默认值。

`options` 是一个对象或字符串带有以下默认值：

``` javascript
{
    flags: 'r',
    encoding: null,
    fd: null,
    mode: 0 o666,
    autoClose: true
}
```

`options` 可以包括 `start` 和 `end` 值，从文件而不是整个文件读取字节范围。`start` 和 `end` 都需要包括在内，并且起始值都是 0。`encoding` 可以是任何可以被 [Buffer](./buffer/buffer.md#) 接受的值。

如果指定了 `fd`，`ReadStream` 会忽略 `path` 参数并且将使用指定的文件描述符。这也意味着不会触发 `'open'` 事件。请注意，`fd` 应该是阻塞的；非阻塞的 `fd` 们应该传给 [net.Socket](./net/class_net_Socket.md#)。

如果 `autoClose` 是 `false`，那么文件描述符将不能被关闭，即使有错误。将其关闭是你的职责，并且需要确保没有文件描述符泄漏。如果 `autoClose` 被设置为 true（默认行为），在 `error` 或 `end` 时，文件描述符将被自动关闭。

`mode` 用于设置文件模式（权限和粘滞位），但仅在创建该文件时有效。

一个例子来读取 100 字节长的文件的最后 10 个字节：

``` javascript
fs.createReadStream('sample.txt', {start: 90, end: 99});
```

如果 `options` 是一个字符串，那么它指定了编码。


<div id="read" class="anchor"></div>
## fs.read(fd, buffer, offset, length, position, callback)

从 `fd` 指定的文件中读取数据。

`buffer` 是该数据将被写入到的 buffer。

`offset` 是在 buffer 中开始写入的偏移量。

`length` 是一个整数，指定要读取的字节的数目。

`position` 是一个整数，指定从该文件中开始读取的位置。如果 `position` 是 `null`，数据将从当前文件位置读取。

回调给出的三个参数 `(err, bytesRead, buffer)`。