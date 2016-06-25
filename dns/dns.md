# 方法和属性

* [dns.setServers(servers)](#setServers)
* [dns.getServers()](#getServers)
* [dns.resolve(hostname[, rrtype], callback)](#resolve)
* [dns.resolve4(hostname, callback)](#resolve4)
* [dns.resolve6(hostname, callback)](#resolve6)
* [dns.resolveMx(hostname, callback)](#resolveMx)
* [dns.resolveTxt(hostname, callback)](#resolveTxt)
* [dns.resolveSrv(hostname, callback)](#resolveSrv)
* [dns.resolveNs(hostname, callback)](#resolveNs)
* [dns.resolveCname(hostname, callback)](#resolveCname)
* [dns.resolveSoa(hostname, callback)](#resolveSoa)
* [dns.reverse(ip, callback)](#reverse)
* [dns.lookup(hostname[, options], callback)](#lookup)
 - [支持的 getaddrinfo 标志](#supported_getaddrinfo_flags)
* [dns.lookupService(address, port, callback)](#lookupService)

--------------------------------------------------


<div id="setServers" class="anchor"></div>
## dns.setServers(servers)

设置解析时要使用的服务器的 IP 地址。`servers` 参数是一组 IPv4 或 IPv6 地址。

如果在一个地址上指定了端口，则该端口会被忽略。

如果提供了无效的地址将会抛出一个错误。

`dns.setServers()` 方法一定不能在 DNS 查询过程中调用。


<div id="getServers" class="anchor"></div>
## dns.getServers()

返回一组正被用于名称解析的 IP 地址字符串。


<div id="resolve" class="anchor"></div>
## dns.resolve(hostname[, rrtype], callback)

使用 DNS 协议解析主机名（如，`'nodejs.org'`）到由 `rrtype` 指定类型的一组记录。

有效的 `rrtype` 值包括：

* `'A'` - IPV4 地址，默认

* `'AAAA'` - IPV6 地址

* `'MX'` - 邮件交换记录

* `'TXT'` - 文本记录

* `'SRV'` - SRV 记录

* `'PTR'` - 用于逆向 IP 查找

* `'NS'` - 名称服务器记录

* `'CNAME'` - 规范名称记录

* `'SOA'` - 规范记录的开头

`callback` 函数有参数 `(err, addresses)`。如果成功，`addresses` 会是一个数组。在 `addresses` 中的每个项目的类型是由记录类型确定，并且描述在相应查找方法的文档中。

当发生错误时，`err` 是一个 [Error](../errors/class_Error.md#) 对象，`err.code` 会是列举在[这里](./error_codes.md#)的错误码的一种。


<div id="resolve4" class="anchor"></div>
## dns.resolve4(hostname, callback)

使用 DNS 协议将 `hostname` 解析为 IPv4 地址（`A` 记录）。传递给 `callback` 函数的 `addresses` 参数会包含一组 IPv4 地址（如，`['74.125.79.104', '74.125.79.105', '74.125.79.106']`）。


<div id="resolve6" class="anchor"></div>
## dns.resolve6(hostname, callback)

使用 DNS 协议将 `hostname` 解析为 IPv6 地址（`AAAA` 记录）。传递给 `callback` 函数的 `addresses` 参数会包含一组 IPv6 地址。


<div id="resolveMx" class="anchor"></div>
## dns.resolveMx(hostname, callback)

使用 DNS 协议将 `hostname` 解析为邮件交换记录（`MX` 记录）。传递给 `callback` 函数的 `addresses` 参数会包含一组带有 `priority` 和 `exchange` 属性的对象（如，`[{priority: 10, exchange: 'mx.example.com'}, ...]`）。


<div id="resolveTxt" class="anchor"></div>
## dns.resolveTxt(hostname, callback)

使用 DNS 协议将 `hostname` 解析为文本查询（`TXT` 记录）。传递给 `callback` 函数的 `addresses` 参数是一个适用于 `hostname` 的二维阵列文本记录（如，`[['v=spf1 ip4:0.0.0.0 ', '~all']]`）。每个子阵列包含一个记录的文本块（TXT chunks）。根据使用案例，这些可能被拼接在一起或分别处理。


<div id="resolveSrv" class="anchor"></div>
## dns.resolveSrv(hostname, callback)

使用 DNS 协议将 `hostname` 解析为服务记录（`SRV` 记录）。传递给 `callback` 函数的 `addresses` 参数会是一组由以下属性构成的对象：

* `priority`

* `weight`

* `port`

* `name`

```javascript
{
    priority: 10,
    weight: 5,
    port: 21223,
    name: 'service.example.com'
}
```


<div id="resolveNs" class="anchor"></div>
## dns.resolveNs(hostname, callback)

使用 DNS 协议将 `hostname` 解析为名称服务器记录（`NS` 记录）。传递给 `callback` 函数的 `addresses` 参数会包含一组适用于 `hostname` 的名称服务器记录（如，`['ns1.example.com', 'ns2.example.com']`）。


<div id="resolveCname" class="anchor"></div>
## dns.resolveCname(hostname, callback)

使用 DNS 协议将 `hostname` 解析为 `CNAME` 记录。传递给 `callback` 函数的 `addresses` 参数会包含一组适用于 `hostname` 的规范名称记录（如，`['bar.example.com']`）。


<div id="resolveSoa" class="anchor"></div>
## dns.resolveSoa(hostname, callback)

使用 DNS 协议将 `hostname` 解析为一个规范记录的开头（`SOA` 记录）。传递给 `callback` 函数的 `addresses` 参数会是一组由以下属性构成的对象：

* `nsname`

* `hostmaster`

* `serial`

* `refresh`

* `retry`

* `expire`

* `minttl`

```javascript
{
    nsname: 'ns.example.com',
    hostmaster: 'root.example.com',
    serial: 2013101809,
    refresh: 10000,
    retry: 2400,
    expire: 604800,
    minttl: 3600
}
```


<div id="reverse" class="anchor"></div>
## dns.reverse(ip, callback)

执行反向 DNS 查询将 IPv4 或 IPv6 地址解析为一组主机名。

`callback` 函数有参数 `(err, hostnames)`。`hostnames` 是一组用给定的 `ip` 解析后的主机名。

当发生错误时，`err` 是一个 [Error](../errors/class_Error.md#) 对象，`err.code` 是 [DNS 错误码](./error_codes.md#) 中的一个。


<div id="lookup" class="anchor"></div>
## dns.lookup(hostname[, options], callback)

解析主机名（如，`'nodejs.org'`）到第一个找到的 A（IPv4）或 AAAA（IPv6）记录。`options` 可以是一个对象或整数。如果没有提供 `options`，那么 IPv4 和 IPv6 地址都是有效的。如果 `options` 是一个整数，那么它必须是 `4` 或 `6`。

另外，`options` 可以是包含这些属性的对象：

* `family`： {Number} - 记录族。如果存在的话，必须是整数 `4` 或 `6`。如果没有提供，IPv4 和 IPv6 的地址都可以接受。

* `hints`： {Number} - 如果存在的话，它应该是一种或多种被支持的 `getaddrinfo` 标志。如果没有提供 `hints`，那么没有标志会传递给 `getaddrinfo`。给 `hints` 的多个标志通过逻辑或（OR）运算的值传递。查阅[支持的 getaddrinfo 标志](#supported_getaddrinfo_flags) 获取更多有关受支持的标志信息。

* `all`：{Boolean} 当为 `true` 时，回调函数会在一个数组里返回所有解析后的地址，否则返回一个单一地址。默认为 `false`。

所有属性都是可选的。选项的使用示例如下所示。

```javascript
{
    family: 4,
    hints: dns.ADDRCONFIG | dns.V4MAPPED,
    all: false
}
```

`callback` 函数有参数 `(err, address, family)`。`address` 是一个 IPv4 或 IPv6 地址的字符串表示。`family` 要么是整数 `4` 或 `6` 并代表着 `address` 族（不一定是最初传给 `lookup` 的值）。

若将选项 `all` 设为 `true`，参数变为 `(err, addresses)`，`addresses` 变成一组由 `address` 和 `family` 属性组成的对象。

当发生错误时，`err` 是一个 [Error](../errors/class_Error.md#) 对象，其中 `err.code` 是错误码。请记住 `err.code` 被设定为 `'ENOENT'` 的情况不局限于域名不存在，也可能在以其他方式查找失败，比如没有可用文件描述符时。

`dns.lookup()` 不一定要在 DNS 协议上做些什么。使用一个操作系统工具的实现，可以与地址名称相关联，反之亦然。此实现可能对任何 Node.js 程序的行为产生微妙但重要的后果。请在使用 `dns.lookup()` 前花一些时间来查阅[实现中的注意事项](./implementation_considerations.md#)章节。


<div id="supported_getaddrinfo_flags" class="anchor"></div>
### 支持的 getaddrinfo 标志

以下标志可以传递给 [dns.lookup()](#lookup) 作为提示：

* `dns.ADDRCONFIG`：返回的地址类型是由当前系统所支持的地址类型决定的。例如，IPv4 地址仅在当前系统至少配置了一个 IPv4 地址时返回。环回地址不考虑。

* `dns.V4MAPPED`：如果指定了 IPv6 族，但没有找到 IPv6 地址。那么返回映射到 IPv4 的 IPv6 地址。需要注意的是，它不支持某些操作系统（如，FreeBSD 的 10.1 版本）。


<div id="lookupService" class="anchor"></div>
## dns.lookupService(address, port, callback)

使用 `getnameinfo` 的操作系统的底层实现来解析给定的 `address` 和 `port` 为主机名和服务器。

`callback` 函数有参数 `(err, hostname, service)`。`hostname` 和 `service` 参数是字符串（例如，分别为 `'localhost'` 和 `'http'`）。

当发生错误时，`err` 是一个 [Error](../errors/class_Error.md#) 对象，其中 `err.code` 是错误码。

```javascript
const dns = require('dns');
dns.lookupService('127.0.0.1', 22, (err, hostname, service) => {
    console.log(hostname, service);
    // Prints: localhost ssh
});
```