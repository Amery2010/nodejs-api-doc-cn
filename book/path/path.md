# 方法和属性

### 属性

* [path.delimiter](#delimiter)
* [path.sep](#sep)
* [path.posix](#posix)
* [path.win32](#win32)

### 方法

* [path.resolve([from ...], to)](#resolve)
* [path.join([path1][, path2][, ...])](#join)
* [path.format(pathObject)](#format)
* [path.normalize(p)](#normalize)
* [path.parse(pathString)](#parse)
* [path.relative(from, to)](#relative)
* [path.isAbsolute(path)](#isAbsolute)
* [path.dirname(p)](#dirname)
* [path.basename(p[, ext])](#basename)
* [path.extname(p)](#extname)

--------------------------------------------------


<div id="delimiter" class="anchor"></div>
## path.delimiter

平台特定的路径分隔符。`';'` 或 `':'`。

在 *nix 上的例子：

```javascript
console.log(process.env.PATH)
// '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'

process.env.PATH.split(path.delimiter)
// returns ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']
```

在 Windows 上的例子：

```javascript
console.log(process.env.PATH)
// 'C:\Windows\system32;C:\Windows;C:\Program Files\node\'

process.env.PATH.split(path.delimiter)
// returns ['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\node\\']
```


<div id="sep" class="anchor"></div>
## path.sep

平台特定的文件分隔符。`'\\'` 或 `'/'`。

在 *nix 上的例子：

```javascript
'foo/bar/baz'.split(path.sep)
// returns ['foo', 'bar', 'baz']
```

在 Windows 上的例子：

```javascript
'foo\\bar\\baz'.split(path.sep)
// returns ['foo', 'bar', 'baz']
```


<div id="posix" class="anchor"></div>
## path.posix

提供访问前面提到的 `path` 方法，但始终以兼容 posix 的方式进行交互。


<div id="win32" class="anchor"></div>
## path.win32

提供访问前面提到的 `path` 方法，但始终以兼容 win32 的方式进行交互。


<div id="resolve" class="anchor"></div>
## path.resolve([from ...], to)

将 `to` 解析为绝对路径。

从右到左的顺序，如果 `to` 不是绝对于 `from` 参数，则前置 `from` 参数，直到找到一个绝对路径为止。如果在使用所有的 `from` 路径后仍然没有找到绝对路径，将会使用当前工作目录。结果路径已经规范化，除非路径被解析到了根目录，否则会删除结尾的斜杠。`from` 中的非字符串参数会被忽略。

另一种思路, 是把它当成 shell 中的一系列 `cd` 命令。

```javascript
path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')
```

这类似于：

```javascript
cd foo/bar
cd /tmp/file/
cd ..
cd a/../subfile
pwd
```

不同的是，不同的路径不需要存在，也可以是文件。

示例：

```javascript
path.resolve('/foo/bar', './baz')
// returns '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/')
// returns '/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
// if currently in /home/myself/node, it returns
// '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

*注意*：如果 `resolve` 参数有零长度的字符串，那么当前的工作目录将被用来替代它们。


<div id="join" class="anchor"></div>
## path.join([path1][, path2][, ...])

把所有的参数加到一起，并规范生成的路径。

参数必须是字符串。在 v0.8 版本中，非字符串参数会被默认忽略。在 v0.10 及以上版本中，会抛出一个错误。

示例：

```javascript
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..')
// returns '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar')
// throws exception
TypeError: Arguments to path.join must be strings
```

*注意*：与其他路径模块函数不同，如果 `join` 参数有零长度的字符串，它们会被忽略。如果已合并的路径字符串是零长度字符串，那么会返回 `'.'`，它代表了当前的工作目录。