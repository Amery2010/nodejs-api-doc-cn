# ALPN、NPN和SNI

ALPN（应用层协议协商拓展）、NPN（下一个协议协议协商）和 SNI（服务器名称表示）是 TLS 握手扩展：

* ALPN/NPN - 允许多种协议使用一个 TLS 服务器（HTTP、SPDY、HTTP/2）。

* SNI - 允许具有不同 SSL 证书的多个主机名使用一个 TLS 服务器。