# 方法和属性

### 属性

* [path.delimiter](#delimiter)
* [path.sep](#sep)
* [path.posix](#posix)
* [path.win32](#win32)

### 方法

* [path.resolve([from ...], to)](#resolve)
* [path.relative(from, to)](#relative)
* [path.format(pathObject)](#format)
* [path.parse(pathString)](#parse)
* [path.join([path1][, path2][, ...])](#join)
* [path.normalize(p)](#normalize)
* [path.dirname(p)](#dirname)
* [path.basename(p[, ext])](#basename)
* [path.extname(p)](#extname)
* [path.isAbsolute(path)](#isAbsolute)

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

*注意*：如果 `resolve` 参数有零长度字符串，那么当前的工作目录将被用来替代它们。


<div id="relative" class="anchor"></div>
## path.relative(from, to)

从 `from` 解析相对路径到 `to`。

有时，我们有两个绝对路径，我们需要导出从一个到另一个的相对路径。这实际上是反向转换 `path.resolve`，这意味着我们可以看到：

```javascript
path.resolve(from, path.relative(from, to)) == path.resolve(to)
```

示例：

```javascript
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb')
// returns '..\\..\\impl\\bbb'

path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb')
// returns '../../impl/bbb'
```

*注意*：如果 `relative` 参数有零长度字符串，那么当前的工作目录会被用于代替零长度字符串。如果两个路径相同，那么将会返回一个零长度字符串。


<div id="format" class="anchor"></div>
## path.format(pathObject)

从一个对象中返回一个路径字符串。这与 [path.parse](#parse) 相反。

如果 `pathObject` 拥有 `dir` 和 `base` 属性，返回的字符串会是 `dir` 属性的级联加上依赖于平台的路径分隔符以及 `base` 属性。

如果没有提供 `dir` 属性，那么 `root` 属性将被用于当成 `dir` 属性。然而，它会假定 `root` 属性已经以依赖于平台的路径分隔符结束。在这种情况下，返回的字符串将会是 `root` 属性的级联加上 `base` 属性。

如果都没有提供 `dir` 和 `root` 属性，那么返回的字符串将是 `base` 属性的内容。

如果没有提供 `base` 属性，`name` 属性的级联加上 `ext` 属性会被用于当成 `base` 属性。

示例：

一些 Posix 系统上例子：

```javascript
// If `dir` and `base` are provided, `dir` + platform separator + `base`
// will be returned.
path.format({
    dir: '/home/user/dir',
    base: 'file.txt'
});
// returns '/home/user/dir/file.txt'

// `root` will be used if `dir` is not specified.
// `name` + `ext` will be used if `base` is not specified.
// If only `root` is provided or `dir` is equal to `root` then the
// platform separator will not be included.
path.format({
    root: '/',
    base: 'file.txt'
});
// returns '/file.txt'

path.format({
    dir: '/',
    root: '/',
    name: 'file',
    ext: '.txt'
});
// returns '/file.txt'

// `base` will be returned if `dir` or `root` are not provided.
path.format({
    base: 'file.txt'
});
// returns 'file.txt'
```

一个 Windows 上的例子：

```javascript
path.format({
    root : "C:\\",
    dir : "C:\\path\\dir",
    base : "file.txt",
    ext : ".txt",
    name : "file"
})
// returns 'C:\\path\\dir\\file.txt'
```


<div id="parse" class="anchor"></div>
## path.parse(pathString)

从路径字符串返回一个对象。

一个在 *nix 上的例子：

```javascript
path.parse('/home/user/dir/file.txt')
// returns
// {
//    root : "/",
//    dir : "/home/user/dir",
//    base : "file.txt",
//    ext : ".txt",
//    name : "file"
// }
```

一个在 Windows 上的例子：

```javascript
path.parse('C:\\path\\dir\\index.html')
// returns
// {
//    root : "C:\\",
//    dir : "C:\\path\\dir",
//    base : "index.html",
//    ext : ".html",
//    name : "index"
// }
```


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

*注意*：与其他路径模块函数不同，如果 `join` 参数有零长度字符串，它们会被忽略。如果已合并的路径字符串是零长度字符串，那么将会返回 `'.'`，它代表了当前的工作目录。


<div id="normalize" class="anchor"></div>
## path.normalize(p)

规范化一个字符串路径，会考虑 `'..'` 和 `'.'` 部分。

当发现多个斜杠时，它们会由单一的斜杠替代；当路径包含尾斜杠时，它会被保存。在 Windows 上使用反斜杠。

示例：

```javascript
path.normalize('/foo/bar//baz/asdf/quux/..')
// returns '/foo/bar/baz/asdf'
```

*注意*：如果作为参数传递的路径字符串是零长度字符串，那么将会返回 `'.'`，它代表了当前的工作目录。


<div id="dirname" class="anchor"></div>
## path.dirname(p)

返回路径的目录名。类似于 Unix 中的 `dirname` 命令。

示例：

```javascript
path.dirname('/foo/bar/baz/asdf/quux')
// returns '/foo/bar/baz/asdf'
```


<div id="basename" class="anchor"></div>
## path.basename(p[, ext])

返回的路径的最后部分。类似于 Unix 中的 `basename` 命令。

示例：

```javascript
path.basename('/foo/bar/baz/asdf/quux.html')
// returns 'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html')
// returns 'quux'
```


<div id="extname" class="anchor"></div>
## path.extname(p)

返回路径的拓展名，在路径的最后部分中的从最后一个 `'.'` 到结尾的字符串。如果路径的最后部分没有 `'.'` 或它的第一个字符是 `'.'`，那么它会返回一个空字符串。

示例：

```javascript
path.extname('index.html')
// returns '.html'

path.extname('index.coffee.md')
// returns '.md'

path.extname('index.')
// returns '.'

path.extname('index')
// returns ''

path.extname('.index')
// returns ''
```


<div id="isAbsolute" class="anchor"></div>
## path.isAbsolute(path)

判定 `path` 是否是一个绝对路径。无论工作目录在哪，绝对路径将始终解析到相同的位置。

Posix 上的例子：

```javascript
path.isAbsolute('/foo/bar') // true
path.isAbsolute('/baz/..')  // true
path.isAbsolute('qux/')     // false
path.isAbsolute('.')        // false
```

Windows 上的例子：

```javascript
path.isAbsolute('//server')  // true
path.isAbsolute('C:/foo/..') // true
path.isAbsolute('bar\\baz')  // false
path.isAbsolute('.')         // false
```

*注意*：如果作为参数传递的路径字符串是零长度字符串，不像其他路径模块函数，它会保持原样，并返回一个 `false`。