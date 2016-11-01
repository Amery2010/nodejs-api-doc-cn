# 方法和属性

* [url.parse(urlStr[, parseQueryString][, slashesDenoteHost])](#urlparseurlstr-parsequerystring-slashesdenotehost)
* [url.format(urlObj)](#urlformaturlobj)
* [url.resolve(from, to)](#urlresolvefrom-to)

--------------------------------------------------


URL 模块提供了以下方法：

## url.parse(urlStr[, parseQueryString][, slashesDenoteHost])

取一个 URL 字符串，并返回一个对象。

给第二个参数传 `true`，会使用 `querystring` 模块解析查询字符串。如果为 `true`，那么 `query` 属性将总是被指定为一个对象，并且 `search` 属性总是一个（可能为空）字符串。如果 `false`，那么 `query` 属性将不会被解析或解码。默认为 `false`。

给第三个参数传 `true`，将会把 `//foo/bar` 作为 `{ host: 'foo', pathname: '/bar' }` 对待，而不是 `{ pathname: '//foo/bar' }`。默认为 `false`。


## url.format(urlObj)

取一个解析的 URL 对象，并返回一个格式化的 URL 字符串。

这里展示的是格式化过程是如何工作的：

* `href` 会被忽略。

* `path` 会被忽略。

* `protocol` 有无尾 `:`（冒号）都被同等对待。

    - 只要 `host` / `hostname` 存在，`http`、`https`、`ftp`、`gopher`、`file` 协议会被补全后缀 `://`（冒号-斜线-斜线）。
    
    - 其他的协议 `mailto`、`xmpp`、`aim`、`sftp`、`foo` 等，会被补全后缀 `:`（冒号）

* `slashes` 如果协议要求 `://`（冒号-斜线-斜线），设置为 `true`。

    - 只需要对此前未被列为需要斜杠的协议进行设置，如 `mongodb://localhost:8000/`，或假设 `host` / `hostname` 不存在。

* `auth` 如果存在的话，会被使用。

* `hostname` 如果 `host` 不存在的话才会被使用。

* `port` 如果 `host` 不存在的话才会被使用。

* `host` 会被用来代替 `hostname` 和 `port`。

* `pathname` 对有无前导的 `/`（斜线）都一视同仁。

* `query`（对象，详见 `querystring`）如果 `search` 不存在的话，会被使用。

* `search` 会被用来代替 `query`。

    - 对有无前导的 `?`（问号）都一视同仁。

* `hash` 对有无前导的 `#`（井号）都一视同仁。


## url.resolve(from, to)

取一个基础的 URL，和一个链接 URL，并解析它们作为一个浏览器可以理解的一个锚标记。例子：

``` javascript
url.resolve('/one/two/three', 'four')         // '/one/two/four'
url.resolve('http://example.com/', '/one')    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two') // 'http://example.com/two'
```