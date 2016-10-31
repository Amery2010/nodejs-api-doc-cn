# SecurePair类

* ['secure' 事件](#secure-事件)

--------------------------------------------------

通过 tls.createSecurePair 返回。


## 'secure' 事件

一旦该安全对已经成功建立安全的连接，这个事件会从 SecurePair 发出。

作为检测服务器端的 [secureConnection](./class_tls_Server.md#secureconnection-事件) 事件的手段，`pair.cleartext.authorized` 应检查并确认使用的证书是否被正确授权。