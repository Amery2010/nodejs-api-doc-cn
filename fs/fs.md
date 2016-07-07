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