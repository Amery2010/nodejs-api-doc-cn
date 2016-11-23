# 注意

* [旧的流 API（Node.js v0.10 之前的版本）](#旧的流-apinodejsv010-之前的版本)
* [近期 ECDH 的变化](#近期-ECDH-的变化)
* [对弱的或泄密的算法的支持](#对弱的或泄密的算法的支持)

--------------------------------------------------


## 旧的流 API（Node.js v0.10 之前的版本）

在存在统一的 Stream API 的概念之前，Crypto 模块已添加到 Node.js 中，并在之前用 [Buffer](../buffer/) 对象处理二进制数据。因此，许多 `crypto` 定义的类有通常在其他由 [streams]() API 实现的 Node.js 类（如，`update()`、`final()` 或 `digest()`）中找不到的方法。此外，许多方法接受和返回 `'binary'` 编码的字符串，而不是 [Buffer](../buffer/)。这个默认值在 Node.js v0.8 之后改为默认使用 [Buffer](../buffer/) 对象。


## 近期 ECDH 的变化

非动态生成的密钥对的 ECDH 的用法已被简化。如今，[ecdh.setPrivateKey()](./class_ECDH.md#ecdhsetprivatekeyprivatekey-encoding) 可以使用预选的私钥来调用，并且相关联的公共点（密钥）将被计算并存储在对象中。这允许代码仅存储和提供 EC 密钥对的私有部分。[ecdh.setPrivateKey()](./class_ECDH.md#ecdhsetprivatekeyprivatekey-encoding) 现在还可以验证私钥对所选曲线的有效性。

[ecdh.setPublicKey()](./class_ECDH.md#ecdhsetpublickeypublickey-encoding) 方法现在已被废弃，因为把它包含在 API 中的并没有什么用处。应该设置先前存储的私钥，或者调用 [ecdh.generateKeys()](./class_ECDH.md#ecdhgeneratekeysencoding-format)。使用[ecdh.setPublicKey()](./class_ECDH.md#ecdhsetpublickeypublickey-encoding) 的主要缺点是它可能会使得 ECDH 密钥对处于不一致的状态。


## 对弱的或泄密的算法的支持

`crypto` 模块仍然支持一些已经泄密和目前不推荐使用的算法。API 还允许使用具有较小密钥大小的密码和散列，这些密钥和散列被认为太弱而无法安全使用。

用户应根据其安全要求，对选择的加密算法和密钥大小负全责。

基于 [NIST SP 800-131A](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-131Ar1.pdf) 的建议：

* MD5 和 SHA-1 在需要抗冲突性的场合（例如，数字签名）时将不可接受。

* 用于 RSA、DSA 和 DH 算法的密钥建议至少 2048 位，并且 ECDSA 和 ECDH 的曲线至少为 224 位，这样的话，在几年内可以安全使用。

* `modp1`、`modp2` 和 `modp5` 的 DH 组的密钥大小小于 2048 位，不推荐使用。

有关其他建议和详细信息，请详见[参考](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-131Ar1.pdf)。