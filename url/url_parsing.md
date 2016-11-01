# URL解析

* [转义字符](#转义字符)

--------------------------------------------------

解析的 URL 对象有下列部分或全部的字段，取决于它们是否存在于 URL 字符串中。不在 URL 字符串中的其他部分将不会出现在所解析的对象中。例子所展示的 URL：

```
`'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`。
```

* `href`：原始解析的完整 URL。无论是协议还是主机都是小写。

例子：`'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`

* `protocol`：请求的协议，小写。

例子：`'http:'`

* `slashes`：该协议要求冒号后斜线。

例子：`true` 或 `false`

* `host`：全部小写的 URL 主机部分，包括端口信息。

例子：`'host.com:8080'`

* `auth`：一个 URL 的认证信息部分。

例子：`'user:pass'`

* `hostname`：只是主机中小写的主机名部分。

例子：`'host.com'`

* `port`：主机的端口号部分。

例子：`'8080'`

* `pathname`：URL 的路径部分，它处于主机之后，查询之前，包括初始的斜线，如果存在的话。不执行解码。

例子：`'/p/a/t/h'`

* `search`：URL 的“查询字符串”部分，包括前导的 `?`。

例子：`'?query=string'`

* `path`：`pathname` 和 `search` 级联。不执行解码。

例子：`'/p/a/t/h?query=string'`

* `query`：'参数'查询字符串的一部分，或一个已解析的查询字符串对象。

例子：`'query=string'` 或 `{'query':'string'}`

* `hash`：URL 的“片段”部分，包括前导的 `#`。

例子：`'#hash'`


## 转义字符

空格（`' '`）和以下字符会在 URL 对象的属性中被自动解析：

```
< > " ` \r \n \t { } | \ ^ '
```