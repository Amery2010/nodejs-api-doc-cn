# TLS/SSL(TLS/SSL)

> 稳定度：2 - 稳定

使用 `require('tls')` 访问这个模块。

`tls` 模块使用 OpenSSL 提供传输层安全性和/或安全套接字层：加密流传输。

TLS/SSL 是一个公开/私有密钥的底层结构。每个客户端和每个服务器必须有一个私钥。创建的私钥是这样的：

``` bash
openssl genrsa -out ryans-key.pem 2048
```

所有服务器和一些客户端需要有一个证书。证书是由证书颁发机构签署或自签名的公钥。第一步，获得证书是创建一个“证书签名请求”（CSR）文件。通过这样做：

``` bash
openssl req -new -sha256 -key ryans-key.pem -out ryans-csr.pem
```

要创建一个 CSR 自签名证书，可以这样做：

``` bash
openssl x509 -req -in ryans-csr.pem -signkey ryans-key.pem -out ryans-cert.pem
```

另外，你可以发送 CSR 到证书颁发机构进行签名。

对于完全正向加密，需要生成 Diffie-Hellman 参数：

``` bash
openssl pkcs12 -export -in agent5-cert.pem -inkey agent5-key.pem -certfile ca-cert.pem -out agent5.pfx
```

* `in`：证书

* `inkey`：私钥

* `certfile`：所有的 CA 证书串连在一个文件中，如 `cat ca1-cert.pem ca2-cert.pem > ca-cert.pem`