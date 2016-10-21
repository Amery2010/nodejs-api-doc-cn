# 缓解由客户端发起的重新协商攻击

TLS 协议允许客户端重新协商 TLS 会话的某些方面。不幸的是，会话重新协商需要非等比的服务器端的资源，这使得它成为拒绝服务攻击的潜在媒介。

为了减轻这个的影响，重新协商被限制为每 10 分钟三次。当超过该阈值时，[tls.TLSSocket](./class_tls_TLSSocket.md#) 实例会发出一个错误。这些限制是可配置的：

* `tls.CLIENT_RENEG_LIMIT`：重新协商次数限制，默认为 3。

* `tls.CLIENT_RENEG_WINDOW`：重新协商窗口存活时间（以秒为单位），默认为 10 分钟。

不要在没有充分理解的情况下去修改这些默认参数。

测试服务器，使用 `openssl s_client -connect address:port` 连接并键入 `R<CR>`（即，小写的 `R` 后跟着回车键）几次。