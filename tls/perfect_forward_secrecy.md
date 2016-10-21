# PFS(完全正向加密)

术语“正向加密”或“完全正向加密”描述密钥协议（即，密钥交换）方法的一个特点。实际上这意味着即使一台服务器的私有密钥被泄露，如果他们设法获得专门为每个会话生成的密钥对，通信只能由窃听者进行解密。

这是通过为每个握手密钥协定随机生成密钥对（与使用所有会话相同的密钥）来实现的。实现该技术的方法被叫做“ephemeral”，从而提供完全正向加密。

目前这两种方法常用于实现完全正向加密（注意字符“E”追加到传统的缩写）：

* [DHE](https://zh.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)：一个 ephemeral 版本的 Diffie Hellman 密钥协商协议。

* [ECDHE](https://en.wikipedia.org/wiki/Elliptic_curve_Diffie%E2%80%93Hellman)：一个 ephemeral 版本的 Elliptic Curve Diffie Hellman 密钥协商协议。

Ephemeral 方法可能有一些性能缺陷，因为密钥生成是昂贵的。