# 修改TLS的默认加密方式

Node.js 建立了一套默认的启用和禁用 TLS 加密套件。当前，默认的加密套件是：

```
ECDHE-RSA-AES128-GCM-SHA256:
ECDHE-ECDSA-AES128-GCM-SHA256:
ECDHE-RSA-AES256-GCM-SHA384:
ECDHE-ECDSA-AES256-GCM-SHA384:
DHE-RSA-AES128-GCM-SHA256:
ECDHE-RSA-AES128-SHA256:
DHE-RSA-AES128-SHA256:
ECDHE-RSA-AES256-SHA384:
DHE-RSA-AES256-SHA384:
ECDHE-RSA-AES256-SHA256:
DHE-RSA-AES256-SHA256:
HIGH:
!aNULL:
!eNULL:
!EXPORT:
!DES:
!RC4:
!MD5:
!PSK:
!SRP:
!CAMELLIA
```

默认值完全可以通过 `--tls-cipher-list` 命令行指令进行覆盖。例如，以下操作使得 `ECDHE-RSA-AES128-GCM-SHA256:!RC4` 成为默认 TLS 加密套件：

``` bash
node --tls-cipher-list="ECDHE-RSA-AES128-GCM-SHA256:!RC4"
```

请注意，包含在 Node.js 内的默认加密套件是经过精心挑选的，反映了当前的安全最佳实践并降低了风险。改变默认的加密套件会对一个应用程序的安全性产生显著影响。`--tls-cipher-list` 指令应该只有在有绝对必要的情况下使用。