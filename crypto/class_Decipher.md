# Decipher类

* [decipher.setAAD(buffer)](#deciphersetaadbuffer)
* [decipher.setAutoPadding(auto_padding=true)](#deciphersetautopaddingautopaddingtrue)
* [decipher.setAuthTag(buffer)](#deciphersetauthtagbuffer)
* [decipher.update(data[, input_encoding][, output_encoding])](#decipherupdatedata-inputencoding-outputencoding)
* [decipher.final([output_encoding])](#decipherfinaloutputencoding)

--------------------------------------------------

`Decipher` 类的实例用于解密数据。该类可以以两种方式进行使用：

* 作为可读和可写的[流](../stream/)，写入简单的加密数据并在可读端上产生未加密数据；

* 使用 [decipher.update()](#decipherupdatedata-inputencoding-outputencoding) 和 [decipher.final()](#decipherfinaloutputencoding) 方法来产生加密数据。

[crypto.createDecipher()](./crypto.md#cryptocreatedecipheralgorithm-password) 和 [crypto.createDecipheriv()](./crypto.md#cryptocreatedecipherivalgorithm-key-iv) 方法用于创建 `Decipher` 实例。`Decipher` 对象无法直接使用 `new` 关键词创建。

示例：将 `Decipher` 对象用作流：

``` javascript
const crypto = require('crypto');
const decipher = crypto.createDecipher('aes192', 'a password');

var decrypted = '';
decipher.on('readable', () => {
    var data = decipher.read();
    if (data)
        decrypted += data.toString('utf8');
});
decipher.on('end', () => {
    console.log(decrypted);
    // Prints: some clear text data
});

var encrypted = 'ca981be48e90867604588e75d04feabb63cc007a8f8ad89b10616ed84d815504';
decipher.write(encrypted, 'hex');
decipher.end();
```

示例：使用 `Decipher` 并导流：

``` javascript
const crypto = require('crypto');
const fs = require('fs');
const decipher = crypto.createDecipher('aes192', 'a password');

const input = fs.createReadStream('test.enc');
const output = fs.createWriteStream('test.js');

input.pipe(decipher).pipe(output);
```

示例：使用 [decipher.update()](#decipherupdatedata-inputencoding-outputencoding) 和 [decipher.final()](#decipherfinaloutputencoding) 方法：

``` javascript
const crypto = require('crypto');
const decipher = crypto.createDecipher('aes192', 'a password');

var encrypted = 'ca981be48e90867604588e75d04feabb63cc007a8f8ad89b10616ed84d815504';
var decrypted = decipher.update(encrypted, 'hex', 'utf8');
decrypted += decipher.final('utf8');
console.log(decrypted);
// Prints: some clear text data
```


## decipher.setAAD(buffer)

使用验证加密模式（目前仅支持 `GCM`）时，`decipher.setAAD()` 方法设置的值用于*附加认证数据*（AAD）输入参数。


## decipher.setAutoPadding(auto_padding=true)

当数据已经被加密而没有标准块填充时，调用 `decipher.setAutoPadding(false)` 将禁用自动填充防止 [decipher.final()](#decipherfinaloutputencoding) 检查和删除填充。

关闭自动填充功能仅在输入数据的长度是加密块大小的倍数时有效。

`decipher.setAutoPadding()` 方法必须在 [decipher.update()](#decipherupdatedata-inputencoding-outputencoding) 之前调用。


## decipher.setAuthTag(buffer)

使用验证加密模式（目前仅支持 `GCM`）时，`decipher.setAuthTag()` 方法用于传入接收的*认证标签*。如果没有提供标签，或者如果密文已经被篡改，[decipher.final()](#decipherfinaloutputencoding) 会抛出由于认证失败而应丢弃密文的指示。


## cipher.update(data[, input_encoding][, output_encoding])

用 `data` 更新解密内容。如果给定了 `input_encoding` 参数，它的值必须是 `'utf8'`、`'ascii'` 或 `'binary'` 其中之一并且 `data` 参数是使用指定编码的字符串。如果没有给定 `input_encoding` 参数，`data` 必须是一个 [Buffer](../buffer/)。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

`output_encoding` 指定加密数据的输出格式，可以是 `'binary'`、`'base64'` 或 `'hex'`。如果指定了 `output_encoding`，将返回使用指定编码的字符串。如果没有提供 `output_encoding`，将返回一个 [Buffer](../buffer/)。

`decipher.update()` 方法可以在新数据上多次调用，直到调用 [decipher.final()](#decipherfinaloutputencoding)。在 [decipher.final()](#decipherfinaloutputencoding) 之后调用 `decipher.update()` 将导致抛出错误。


## cipher.final([output_encoding])

返回任何剩余的解密内容。如果 `output_encoding` 参数是 `'binary'`、`'base64'` 或 `'hex'` 中的一个，将返回一个字符串。如果没有提供 `output_encoding`，将返回一个 [Buffer](../buffer/)。

一旦调用了 `decipher.final()` 方法，`Decipher` 对象不能再用于解密数据。尝试多次调用 `decipher.final()` 将导致抛出错误。