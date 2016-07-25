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
* [fs.stat(path, callback)](#stat)
* [fs.statSync(path)](#statSync)
* [fs.chmod(path, mode, callback)](#chmod)
* [fs.chmodSync(path, mode)](#chmodSync)
* [fs.chown(path, uid, gid, callback)](#chown)
* [fs.chownSync(path, uid, gid)](#chownSync)
* [fs.utimes(path, atime, mtime, callback)](#utimes)
* [fs.utimesSync(path, atime, mtime)](#utimesSync)
* [fs.exists(path, callback)](#exists)
* [fs.existsSync(path)](#existsSync)
* [fs.open(path, flags[, mode], callback)](#open)
* [fs.openSync(path, flags[, mode])](#openSync)
* [fs.close(fd, callback)](#close)
* [fs.closeSync(fd)](#closeSync)
* [fs.access(path[, mode], callback)](#access)
* [fs.accessSync(path[, mode])](#accessSync)
* [fs.rename(oldPath, newPath, callback)](#rename)
* [fs.renameSync(oldPath, newPath)](#renameSync)
* [fs.watch(filename[, options][, listener])](#watch)
  - [注意事项](#caveats)
    - [可用性](#availability)
    - [索引节点](#inodes)
    - [文件名参数](#filename_argument)
* [fs.watchFile(filename[, options], listener)](#watchFile)
* [fs.unwatchFile(filename[, listener])](#unwatchFile)
* [fs.readFile(file[, options], callback)](#readFile)
* [fs.readFileSync(file[, options])](#readFileSync)
* [fs.writeFile(file, data[, options], callback)](#writeFile)
* [fs.writeFileSync(file, data[, options])](#writeFileSync)
* [fs.appendFile(file, data[, options], callback)](#appendFile)
* [fs.appendFileSync(file, data[, options])](#appendFileSync)
* [fs.ftruncate(fd, len, callback)](#ftruncate)
* [fs.ftruncateSync(fd, len)](#ftruncateSync)
* [fs.fstat(fd, callback)](#fstat)
* [fs.fstatSync(fd)](#fstatSync)
* [fs.fchmod(fd, mode, callback)](#fchmod)
* [fs.fchmodSync(fd, mode)](#fchmodSync)
* [fs.fchown(fd, uid, gid, callback)](#fchown)
* [fs.fchownSync(fd, uid, gid)](#fchownSync)
* [fs.futimes(fd, atime, mtime, callback)](#futimes)
* [fs.futimesSync(fd, atime, mtime)](#futimesSync)
* [fs.fsync(fd, callback)](#fsync)
* [fs.fsyncSync(fd)](#fsyncSync)
* [fs.fdatasync(fd, callback)](#fdatasync)
* [fs.fdatasyncSync(fd)](#fdatasyncSync)

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

返回一个新的 [ReadStream](../class_fs_ReadStream.md#) 对象（详见 [可读流章节](../stream/api_for_stream_consumers.md#class_Readable)）。

要知道，不同于在一个可读流上设置的 `highWaterMark` 默认值（16 kb），此方法在相同参数下返回的流具有 64 kb 的默认值。

`options` 是一个对象或字符串带有以下默认值：

``` javascript
{
    flags: 'r',
    encoding: null,
    fd: null,
    mode: 0o666,
    autoClose: true
}
```

`options` 可以包括 `start` 和 `end` 值，从文件而不是整个文件读取字节范围。`start` 和 `end` 都需要包括在内，并且起始值都是 0。`encoding` 可以是任何可以被 [Buffer](../buffer/buffer.md#) 接受的值。

如果指定了 `fd`，`ReadStream` 会忽略 `path` 参数并且将使用指定的文件描述符。这也意味着不会触发 `'open'` 事件。请注意，`fd` 应该是阻塞的；非阻塞的 `fd` 们应该传给 [net.Socket](../net/class_net_Socket.md#)。

如果 `autoClose` 是 `false`，那么文件描述符将不会被关闭，即使有错误。将其关闭是你的职责，并且需要确保没有文件描述符泄漏。如果 `autoClose` 被设置为 true（默认行为），在 `error` 或 `end` 时，文件描述符将被自动关闭。

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


<div id="readSync" class="anchor"></div>
## fs.readSync(fd, buffer, offset, length, position)

同步版的 [fs.read()](#read)。返回 `bytesRead` 的数。


<div id="createWriteStream" class="anchor"></div>
## fs.createWriteStream(path[, options])

返回一个新的 [WriteStream](../class_fs_WriteStream.md#) 对象（详见 [可写流章节](../stream/api_for_stream_consumers.md#class_Writable)）。

`options` 是一个对象或字符串带有以下默认值：

``` javascript
{
    flags: 'w',
    defaultEncoding: 'utf8',
    fd: null,
    mode: 0o666,
    autoClose: true
}
```

`options` 也可以包括一个 `start` 选项以允许在以前的文件开头部分的位置写入数据。如果是修改文件而不是替换它的话可能需要 `r+` 的 `flags` 模式而不是默认的 `w` 模式。`defaultEncoding` 可以是任何可以被 [Buffer](../buffer/buffer.md#) 接受的值。

如果 `autoClose` 被设置为 true（默认行为），在文件描述符发生 `error` 或 `end` 事件时，会被自动关闭。如果 `autoClose` 是 false，那么文件描述符将不会被关闭，即使有错误。将其关闭是你的职责，并且需要确保没有文件描述符泄漏。

类似 [ReadStream](../class_fs_ReadStream.md#)，如果指定了 `fd`，`WriteStream` 会忽略 `path` 参数，并且将使用指定的文件描述符。。这也意味着不会触发 `'open'` 事件。请注意，`fd` 应该是阻塞的；非阻塞的 `fd` 们应该传给 [net.Socket](../net/class_net_Socket.md#)。

如果 `options` 是一个字符串，那么它指定了编码。


<div id="write_data" class="anchor"></div>
## fs.write(fd, data[, position[, encoding]], callback)

写入 `data` 到由 `fd` 指定的文件。如果 `data` 不是一个 Buffer 实例，那么该值将被强制转换为字符串。

`position` 指向该数据应从哪里写入的文件的开头的偏移量。如果 `typeof position !== 'number'` 该数据将在当前位置被写入。详见 [pwrite(2)](http://man7.org/linux/man-pages/man2/pwrite.2.html)。

`encoding` 是预期的字符串编码。

该回调将接收 `(err, written, string)` 参数，`written` 用于指定传递过来的字符串需要被写入多少字节。请注意，写入字节与字符串中的字符不同。详见 [Buffer.byteLength](../buffer/class_Buffer.md#Buffer_byteLength)。

与写入 `buffer` 不同，整个字符串必须写入。无子字符串可以指定。这是因为字节所得到的数据的偏移量可能与字符串的偏移量不同。

需要注意的是使用 `fs.write` 在不等待回调的情况下对同一文件进行多次写入，这是不安全的操作。对于这种情况，我们强烈推荐使用 `fs.createWriteStream` 方法。

在 Linux 上，当文件以追加模式打开时，位置写入不起作用。内核会忽略位置参数，并总是将数据附加到文件的末尾。


<div id="write_data_sync" class="anchor"></div>
## fs.writeSync(fd, data[, position[, encoding]])

同步版的 [fs.write()](#write_data)。返回写入的字节数。


<div id="write_buffer" class="anchor"></div>
## fs.write(fd, buffer, offset, length[, position], callback)

写入 `buffer` 到由 `fd` 指定的文件。

`offset` 和 `length` 确定该 buffer 被写入的部分。

`position` 指向该数据应从哪里写入的文件的开头的偏移量。如果 `typeof position !== 'number'` 该数据将在当前位置被写入。详见 [pwrite(2)](http://man7.org/linux/man-pages/man2/pwrite.2.html)。

该回调会给出三个参数 `(err, written, buffer) `，`written` 用于指定从 `buffer` 中写入多少*字节*。

需要注意的是使用 `fs.write` 在不等待回调的情况下对同一文件进行多次写入，这是不安全的操作。对于这种情况，我们强烈推荐使用 `fs.createWriteStream` 方法。

在 Linux 上，当文件以追加模式打开时，位置写入不起作用。内核会忽略位置参数，并总是将数据附加到文件的末尾。


<div id="write_buffer_sync" class="anchor"></div>
## fs.writeSync(fd, buffer, offset, length[, position])

同步版的 [fs.write()](#write_buffer)。返回写入的字节数。


<div id="truncate" class="anchor"></div>
## fs.truncate(path, len, callback)

异步的 [truncate(2)](http://man7.org/linux/man-pages/man2/truncate.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。文件描述符也可以作为第一个参数传递。在这种情况下，`fs.ftruncate()` 被调用。


<div id="truncateSync" class="anchor"></div>
## fs.truncateSync(path, len)

同步的 [truncate(2)](http://man7.org/linux/man-pages/man2/truncate.2.html)。返回 `undefined`。


<div id="stat" class="anchor"></div>
## fs.stat(path, callback)

异步的 [stat(2)](http://man7.org/linux/man-pages/man2/stat.2.html)。回调有两个参数 `(err, stats)`，其中 `stats` 是一个 [fs.Stats](../class_fs_Stats.md#) 对象。详见 [fs.Stats](../class_fs_Stats.md#) 章节了解更多信息。


<div id="statSync" class="anchor"></div>
## fs.statSync(path)

同步的 [stat(2)](http://man7.org/linux/man-pages/man2/stat.2.html)。返回一个 [fs.Stats](../class_fs_Stats.md#) 实例。


<div id="chmod" class="anchor"></div>
## fs.chmod(path, mode, callback)

异步的 [chmod(2)](http://man7.org/linux/man-pages/man2/chmod.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="chmodSync" class="anchor"></div>
## fs.chmodSync(path, mode)

同步的 [chmod(2)](http://man7.org/linux/man-pages/man2/chmod.2.html)。返回 `undefined`。


<div id="chown" class="anchor"></div>
## fs.chown(path, uid, gid, callback)

异步的 [chown(2)](http://man7.org/linux/man-pages/man2/chown.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="chownSync" class="anchor"></div>
## fs.chownSync(path, uid, gid)

同步的 [chown(2)](http://man7.org/linux/man-pages/man2/chown.2.html)。返回 `undefined`。


<div id="utimes" class="anchor"></div>
## fs.utimes(path, atime, mtime, callback)

改变由 `path` 提供的路径所引用的文件的文件时间戳。

注意：`atime` 和 `mtime` 参数跟随的相关函数是否遵循以下规则：

* 如果值是一个像 `'123456789'` 那样的可以数值化的字符串，该值会得到转换为对应的数值。

* 如果值是 `NaN` 或 `Infinity`，该值将被转换为 `Date.now()`。


<div id="utimesSync" class="anchor"></div>
## fs.utimesSync(path, atime, mtime)

同步版的 [fs.utimes()](#utimes)。返回 `undefined`。


<div id="exists" class="anchor"></div>
## fs.exists(path, callback)

> 稳定度：0 - 已废弃：使用 [fs.stat()](#stat) 或 [fs.access()](#access) 代替。

通过使用文件系统检查是否存在给定的路径的测试。然后回调给 `callback` 的参数 `true` 或 `false`。例如：

``` javascript
fs.exists('/etc/passwd', (exists) => {
    console.log(exists ? 'it\'s there' : 'no passwd!');
});
```

在调用 `fs.open()` 前，`fs.exists()` 不应该被用于检测文件是否存在。这样做会形成一种竞争状态，因为其他进程可能会在两个调用之间改变文件的状态。反而，用户代码应直接调用 `fs.open()` 并处理如果该文件不存在引发的错误。


<div id="existsSync" class="anchor"></div>
## fs.existsSync(path)

> 稳定度：0 - 已废弃：使用 [fs.statSync()](#statSync) 或 [fs.accessSync()](#accessSync) 代替。

同步版的 [fs.exists()](#exists)。如果文件存在则返回 `true`，否则返回 `false`。


<div id="open" class="anchor"></div>
## fs.open(path, flags[, mode], callback)

异步打开文件。详见 [open(2)](http://man7.org/linux/man-pages/man2/open.2.html)。

`flags` 可以是：

* `'r'` - 以读取模式打开文件。如果该文件不存在会发生异常。

* `'r+'` - 以读写模式打开文件。如果该文件不存在会发生异常。

* `'rs'` - 以同步读取模式打开文件。指示操作系统绕过本地文件系统的高速缓存。

    这对 NFS 挂载模式下打开文件很有用，因为它可以让你跳过潜在的陈旧的本地缓存。它会对 I/O 的性能产生明显地影响，所以除非你需要，否则请不要使用此标志。
    
    请注意，这不会转换 `fs.open()` 进入同步阻塞调用。如果这是你想要的，那么你应该使用 `fs.openSync()`。
    
* `'rs+'` - 以读写模式打开文件，告知操作系统同步打开。详见 `'rs'` 中的有关的注意点。

* `'w'` - 以写入模式打开文件。将被创建（如果文件不存在）或截断（如果文件存在）。

* `'wx'` - 类似于 `'w'`，但如果 `path` 存在，将会导致失败。

* `'w+'` - 以读写模式打开文件。将被创建（如果文件不存在）或截断（如果文件存在）。

* `'wx+'` - 类似于 `'w+'`，但如果 `path` 存在，将会导致失败。

* `'a'` - 以追加模式打开文件。如果不存在，该文件会被创建。

* `'ax'` - 类似于 `'a'`，但如果 `path` 存在，将会导致失败。

* `'a+'` - 以读取和追加模式打开文件。如果不存在，该文件会被创建。

* `'ax+'` - 类似于 `'a+'`，但如果 `path` 存在，将会导致失败。

`mode` 设置文件模式（权限和粘滞位），但只有当文件被创建时有效。它默认为 `0666`，可读写。

该回调有两个参数 `(err, fd)`。

独家标志 `'x'`（在 [open(2)](http://man7.org/linux/man-pages/man2/open.2.html) 中的 `O_EXCL` 标志）确保 `path` 是新创建的。在 POSIX 操作系统中，哪怕是一个符号链接到一个不存在的文件，`path` 也会被视为存在。独家标志有可能可以在网络文件系统中工作。

`flags` 也可以是一个记录在 [open(2)](http://man7.org/linux/man-pages/man2/open.2.html) 中的数字；常用的辅音可从 `require('constants')` 获取。在 Windows 中，标志被转换为它们等同的替代，可适用如 `O_WRONLY` 到 `FILE_GENERIC_WRITE`，或 `O_EXCL|O_CREAT` 到 `CREATE_NEW` 通过 CreateFileW 接受。

在 Linux 中，当文件以追加模式打开时，位置写入不起作用。内核忽略位置参数，并总是将数据附加到文件的末尾。


<div id="openSync" class="anchor"></div>
## fs.openSync(path, flags[, mode])

同步版的 [fs.open()](#open)。返回表示文件描述符的整数。


<div id="close" class="anchor"></div>
## fs.close(fd, callback)

异步的 [close(2)](http://man7.org/linux/man-pages/man2/close.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="closeSync" class="anchor"></div>
## fs.closeSync(fd)

同步的 [close(2)](http://man7.org/linux/man-pages/man2/close.2.html)。返回 `undefined`。


<div id="access" class="anchor"></div>
## fs.access(path[, mode], callback)

测试由 `path` 指定的文件的用户权限。`mode` 是一个可选的整数，指定要执行的辅助检查。以下常量定义 `mode` 的可能值。有可能产生由位或和两个或更多个值组成的掩码。

* `fs.F_OK` - 文件对调用进程可见。这在确定文件是否存在时很有用，但只字未提 rwx 权限。如果 `mode` 没被指定，默认为该值。

* `fs.R_OK` - 文件可以通过调用进程读取。

* `fs.W_OK` - 文件可以通过调用进程写入。

* `fs.X_OK` - 文件可以通过调用进程执行。这对 Windows 没有影响（行为类似 `fs.F_OK`）。

最后一个参数 `callback` 是一个回调函数，调用时可能带有一个 error 参数。如果有任何辅助检查失败，错误参数将被填充。下面的例子将检查 `/etc/passwd` 文件是否可以被读取并被当前进程写入。

``` javascript
fs.access('/etc/passwd', fs.R_OK | fs.W_OK, (err) => {
    console.log(err ? 'no access!' : 'can read/write');
});
```


<div id="accessSync" class="anchor"></div>
## fs.accessSync(path[, mode])

同步版的 [fs.access()](#access)。如果有任何辅助检查失败将抛出错误，否则什么也不做。


<div id="rename" class="anchor"></div>
## fs.rename(oldPath, newPath, callback)

异步的 [rename(2)](http://man7.org/linux/man-pages/man2/rename.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="renameSync" class="anchor"></div>
## fs.renameSync(oldPath, newPath)

同步的 [rename(2)](http://man7.org/linux/man-pages/man2/rename.2.html)。返回 `undefined`。


<div id="watch" class="anchor"></div>
## fs.watch(filename[, options][, listener])

监视 `filename` 的变化，`filename` 既可以是一个文件也可以是一个目录。返回的对象是一个 [fs.FSWatcher](../class_fs_FSWatcher.md#) 实例。

第二个参数是可选的。如果提供的话，`options` 应该是一个对象。支持的布尔成员是 `persistent` 和 `recursive`。`persistent` 表示当文件被监视时，该进程是否应该持续运行。`recursive` 表示所有子目录是否应该关注，或仅当前目录。只有在支持的平台（参见[注意事项](#caveats)）上可以指定目录适用。

默认为 `{ persistent: true, recursive: false }`。

该监听回调获得两个参数 `(event, filename)`。`event` 既可以是 `'rename'` 也可以是 `'change'`，并且 `filename` 是其中触发事件的文件的名称。


<div id="caveats" class="anchor"></div>
### 注意事项

该 `fs.watch` API 不是 100％ 的跨平台一致，并且在某些情况下不可用。

递归选项只支持 OS X 和 Windows。


<div id="availability" class="anchor"></div>
#### 可用性

此功能依赖于底层操作系统提供的一种方法来通知文件系统的变化。

* 在 Linux 系统中，使用 `inotify`。

* 在 BSD 系统中，使用 `kqueue`。

* 在 OS X 系统中，对文件使用 `kqueue`，对目录使用 `'FSEvents'`。

* 在 SunOS 系统（包括 Solaris 和 SmartOS）中，使用 `event ports`。

* 在 Windows 系统中，此功能取决于 `ReadDirectoryChangesW`。

如果底层功能出于某种原因不可用，那么 `fs.watch` 也将无法正常工作。例如，在网络文件系统（ZFS，SMB 等）中监视文件或目录往往并不可靠或都不可靠。

你可以仍然使用 `fs.watchFile`，它使用状态查询，但它较慢和不可靠。


<div id="inodes" class="anchor"></div>
#### 索引节点

在 Linux 或 OS X 系统中，`fs.watch()` 解析路径到一个[索引节点](http://www.linux.org/threads/intro-to-inodes.4130)，并监视该索引节点。如果监视的路径被删除或重建，它会被赋予一个新的索引节点。监视器会发出一个删除事件，但会继续监视该*原始*索引节点。新建索引节点的事件不会被触发。这是正常现象。


<div id="filename_argument" class="anchor"></div>
#### 文件名参数

在回调中提供 `filename` 参数，仅在 Linux 和 Windows 系统上支持。即使在支持的平台中，`filename` 也不能保证提供。因此，不要以为 `filename` 参数总是在回调中提供，并在它是 null 的情况下，提供一定的后备逻辑。

``` javascript
fs.watch('somedir', (event, filename) => {
    console.log(`event is: ${event}`);
    if (filename) {
        console.log(`filename provided: ${filename}`);
    } else {
        console.log('filename not provided');
    }
});
```


<div id="watchFile" class="anchor"></div>
## fs.watchFile(filename[, options], listener)

监视 `filename` 的变化。`listener` 回调在每次访问文件时会被调用。

`options` 参数可被省略。如果提供的话，它应该是一个对象。`options` 对象可能包含一个名为 `persistent` 的布尔值，表示当文件被监视时，该进程是否应该持续运行。`options` 对象可以指定一个 `interval` 属性，表示目标多久应以毫秒为单位进行查询。该默认值为 `{ persistent: true, interval: 5007 }`。

`listener` 有两个参数，当前的状态对象和以前的状态对象：

``` javascript
fs.watchFile('message.text', (curr, prev) => {
    console.log(`the current mtime is: ${curr.mtime}`);
    console.log(`the previous mtime was: ${prev.mtime}`);
});
```

这里的状态对象是 `fs.Stat` 的实例。

如果你想在文件被修改时得到通知，不只是访问，你也需要比较 `curr.mtime` 和 `prev.mtime`。

*注意：当 `fs.watchFile` 的运行结果属于一个 `ENOENT` 错误时，它将以所有字段为零（或日期、Unix 纪元）调用监听器一次。在 Windows 中，`blksize` 和 `blocks` 字段会是 `undefined`，而不是零。如果文件是在那之后创建的，监听器会被再次以最新的状态对象进行调用。这是在 v0.10 版之后在功能上的变化。*

*注意：[fs.watch()](#watch) 比 `fs.watchFile` 和 `fs.watchFile` 更高效。可能的话，`fs.watch` 应当被用来代替 `fs.watchFile` 和 `fs.unwatchFile`。*


<div id="unwatchFile" class="anchor"></div>
## fs.unwatchFile(filename[, listener])

停止监视 `filename` 的变化。如果指定了 `listener`，只会移除特定的监视器。否则，*所有*的监视器都会被移除，并且你已经有效地停止监视 `filename`。

带有一个没有被监视的文件名来调用 `fs.unwatchFile()` 是一个空操作，而不是一个错误。

*注意：[fs.watch()](#watch) 比 `fs.watchFile` 和 `fs.watchFile` 更高效。可能的话，`fs.watch` 应当被用来代替 `fs.watchFile` 和 `fs.unwatchFile`。*


<div id="readFile" class="anchor"></div>
## fs.readFile(file[, options], callback)

* `file` {String} | {Integer} 文件名或文件描述符

* `options` {Object} | {String}

  - `encoding` {String} | {Null} 默认 = `null`
  
  - `flag` {String} 默认 = `'r'`
  
* `callback` {Function}

异步读取一个文件的全部内容。例如：

``` javascript
fs.readFile('/etc/passwd', (err, data) => {
    if (err) throw err;
    console.log(data);
});
```

该回调传入两个参数 `(err, data)`，`data` 是文件的内容。

如果没有指定编码，则返回原始 buffer。

如果 `options` 是一个字符串，那么它指定了编码。例如：

``` javascript
fs.readFile('/etc/passwd', 'utf8', callback);
```

任何指定的文件描述符必须支持读取。

*注意：指定的文件描述符不会被自动关闭。*


<div id="readFileSync" class="anchor"></div>
## fs.readFileSync(file[, options])

同步版的 [fs.readFile](#readFile)。返回 `file` 的内容。

如果 `encoding` 参数被指定，则此函数返回一个字符串。否则，它返回一个 buffer。


<div id="writeFile" class="anchor"></div>
## fs.writeFile(file, data[, options], callback)

* `file` {String} | {Integer} 文件名或文件描述符

* `data` {String} | {Buffer}

* `options` {Object} | {String}

  - `encoding` {String} | {Null} 默认 = `'utf8'`
  
  - `mode` {Number} 默认 = `0o666`
  
  - `flag` {String} 默认 = `'w'`
  
* `callback` {Function}

异步将数据写入到文件，如果它已经存在，则替换该文件。`data` 可以是一个字符串或 buffer。

如果 `data` 是一个 buffer，则忽略 `encoding` 选项。它默认为 `'utf8'`。

例子：

``` javascript
fs.writeFile('message.txt', 'Hello Node.js', (err) => {
    if (err) throw err;
    console.log('It\'s saved!');
});
```

如果 `options` 是一个字符串，那么它指定了编码。例如：

``` javascript
fs.writeFile('message.txt', 'Hello Node.js', 'utf8', callback);
```

任何指定的文件描述符必须支持写入。

需要注意的是使用 `fs.writeFile` 在不等待回调的情况下对同一文件进行多次写入，这是不安全的操作。对于这种情况，我们强烈推荐使用 `fs.createWriteStream` 方法。

*注意：指定的文件描述符不会被自动关闭。*


<div id="writeFileSync" class="anchor"></div>
## fs.writeFileSync(file, data[, options])

同步版的 [fs.writeFile()](#writeFile)。返回 `undefined`。


<div id="appendFile" class="anchor"></div>
## fs.appendFile(file, data[, options], callback)

* `file` {String} | {Number} 文件名或文件描述符

* `data` {String} | {Buffer}

* `options` {Object} | {String}

  - `encoding` {String} | {Null} 默认 = `'utf8'`
  
  - `mode` {Number} 默认 = `0o666`
  
  - `flag` {String} 默认 = `'a'`
  
* `callback` {Function}

异步追加数据到文件，如果它已经存在，则替换该文件。`data` 可以是一个字符串或 buffer。

例子：

``` javascript
fs.appendFile('message.txt', 'data to append', (err) => {
    if (err) throw err;
    console.log('The "data to append" was appended to file!');
});
```

如果 `options` 是一个字符串，那么它指定了编码。例如：

``` javascript
fs.appendFile('message.txt', 'data to append', 'utf8', callback);
```

任何指定的文件描述符必须为了追加而被打开。

*注意：指定的文件描述符不会被自动关闭。*


<div id="appendFileSync" class="anchor"></div>
## fs.appendFileSync(file, data[, options])

同步版的 [fs.appendFile()](#appendFile)。返回 `undefined`。


<div id="ftruncate" class="anchor"></div>
## fs.ftruncate(fd, len, callback)

异步的 [ftruncate(2)](http://man7.org/linux/man-pages/man2/ftruncate.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="ftruncateSync" class="anchor"></div>
## fs.ftruncateSync(fd, len)

同步的 [ftruncate(2)](http://man7.org/linux/man-pages/man2/ftruncate.2.html)。返回 `undefined`。


<div id="fstat" class="anchor"></div>
## fs.fstat(fd, callback)

异步的 [fstat(2)](http://man7.org/linux/man-pages/man2/fstat.2.html)。该回调获得两个参数 `(err, stats)`，`stats` 是一个 `fs.Stats` 对象。`fstat()` 与 [fs.stat()](#stat) 类似，除了文件是通过指定的文件描述符 `fd` 来说明。


<div id="fstatSync" class="anchor"></div>
## fs.fstatSync(fd)

同步的 [fstat(2)](http://man7.org/linux/man-pages/man2/fstat.2.html)。返回一个 `fs.Stats` 实例。


<div id="fchmod" class="anchor"></div>
## fs.fchmod(fd, mode, callback)

异步的 [fchmod(2)](http://man7.org/linux/man-pages/man2/fchmod.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="fchmodSync" class="anchor"></div>
## fs.fchmodSync(fd, mode)

同步的 [fchmod(2)](http://man7.org/linux/man-pages/man2/fchmod.2.html)。返回 `undefined`。


<div id="fchown" class="anchor"></div>
## fs.fchown(fd, uid, gid, callback)

异步的 [fchown(2)](http://man7.org/linux/man-pages/man2/fchown.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="fchownSync" class="anchor"></div>
## fs.fchownSync(fd, uid, gid)

同步的 [fchown(2)](http://man7.org/linux/man-pages/man2/fchown.2.html)。返回 `undefined`。


<div id="futimes" class="anchor"></div>
## fs.futimes(fd, atime, mtime, callback)

改变由所提供的文件描述符所引用的文件的文件时间戳。


<div id="futimesSync" class="anchor"></div>
## fs.futimesSync(fd, atime, mtime)

同步版的 [fs.futimes()](https://nodejs.org/dist/latest-v5.x/docs/api/fs.html#fs_fs_futimes_fd_atime_mtime_callback)。返回 `undefined`。


<div id="fsync" class="anchor"></div>
## fs.fsync(fd, callback)

异步的 [fsync(2)](http://man7.org/linux/man-pages/man2/fsync.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="fsyncSync" class="anchor"></div>
## fs.fsyncSync(fd)

同步的 [fsync(2)](http://man7.org/linux/man-pages/man2/fsync.2.html)。返回 `undefined`。


<div id="fdatasync" class="anchor"></div>
## fs.fdatasync(fd, callback)

异步的 [fdatasync(2)](http://man7.org/linux/man-pages/man2/fdatasync.2.html)。除了一个可能的异常参数外没有其他参数会给到完成时的回调。


<div id="fdatasyncSync" class="anchor"></div>
## fs.fdatasyncSync(fd)

同步的 [fdatasync(2)](http://man7.org/linux/man-pages/man2/fdatasync.2.html)。返回 `undefined`。