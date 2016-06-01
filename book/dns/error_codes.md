# 错误代码

每个 DNS 查询都可以返回下列之一的错误代码：

* `dns.NODATA`：DNS 服务器返回没有数据的应答。

* `dns.FORMERR`：DNS 服务器查询所要求的格式不正确。

* `dns.SERVFAIL`：DNS 服务器返回一般故障。

* `dns.NOTFOUND`：未找到域名。

* `dns.NOTIMP`：DNS 服务器不执行请求的操作。

* `dns.REFUSED`：DNS 服务器拒绝查询。

* `dns.BADQUERY`：格式错误的 DNS 查询。

* `dns.BADNAME`：格式错误的主机名。

* `dns.BADFAMILY`：不支持的地址族。

* `dns.BADRESP`：格式错误的 DNS 应答。

* `dns.CONNREFUSED`：无法连接到 DNS 服务器。

* `dns.TIMEOUT`：联系 DNS 服务器超时。

* `dns.EOF`：文件结尾。

* `dns.FILE`：读取文件错误。

* `dns.NOMEM`：内存不足。

* `dns.DESTRUCTION`：通道被销毁。

* `dns.BADSTR`：格式错误的字符串。

* `dns.BADFLAGS`：指定的非法标识。

* `dns.NONAME`：主机名不是数字。

* `dns.BADHINTS`：指定的非法提示标识。

* `dns.NOTINITIALIZED`：还未初始化 c-ares 库。

* `dns.LOADIPHLPAPI`：加载 iphlpapi.dll 失败。

* `dns.ADDRGETNETWORKPARAMS`：无法找到获取网络参数的函数。

* `dns.CANCELLED`：取消 DNS 查询。