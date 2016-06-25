# 域名服务(DNS)

> 稳定度：2 - 稳定

`dns` 模块包含属于两个不同类别的函数：

1）函数使用底层的操作系统设备执行名称解析，并且不一定执行任何类型的网络通信。这个类别只包含一个函数： [dns.lookup()](./dns.md#lookup)。**开发者希望和同一操作系统中的其他应用程序的行为相同的方式执行名称解析应该使用 [dns.lookup()](./dns.md#lookup)**。

例如，监视 `nodejs.org`。

```javascript
const dns = require('dns');

dns.lookup('nodejs.org', (err, addresses, family) => {
    console.log('addresses:', addresses);
});
```

2）函数连接到实际的DNS服务器执行名称解析，并且*总是*使用网络来执行 DNS 查询。此类别包含 `dns` 模块里*除了* [dns.lookup()](./dns.md#lookup) 以外的所有函数。这些函数不使用与 [dns.lookup()](./dns.md#lookup) 相同的配置文件（如，`/etc/hosts`）。这些函数应该由不希望使用底层的操作系统设备执行名称解析的开发者使用，并且希望*始终*执行 DNS 查询。

下面是解析 `'nodejs.org'`，然后反向解析返回的 IP 地址的例子。

```javascript
const dns = require('dns');

dns.resolve4('nodejs.org', (err, addresses) => {
    if (err) throw err;

    console.log(`addresses: ${JSON.stringify(addresses)}`);

    addresses.forEach((a) => {
        dns.reverse(a, (err, hostnames) => {
            if (err) {
                throw err;
            }
            console.log(`reverse for ${a}: ${JSON.stringify(hostnames)}`);
        });
    });
});
```

一个在另一个之上进行选择会有微妙的后果，详情参阅[实施注意事项部分](./implementation_considerations.md#)获取更多信息。