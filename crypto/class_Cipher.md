# Cipher类

* [cipher.setAAD(buffer)](#ciphersetaadbuffer)
* [cipher.setAutoPadding(auto_padding=true)](#ciphersetautopaddingautopaddingtrue)
* [cipher.getAuthTag()](#ciphergetauthtag)
* [cipher.update(data[, input_encoding][, output_encoding])](#cipherupdatedata-inputencoding-outputencoding)
* [cipher.final([output_encoding])](#cipherfinaloutputencoding)

--------------------------------------------------

`Cipher` 类的实例用于加密数据。该类可以以两种方式进行使用：

* 作为可读和可写的[流](../stream/)，写入简单的未加密数据并在可读端上产生加密数据；

* 使用 [cipher.update()](#cipherupdatedata-inputencoding-outputencoding) 和 [cipher.final()](#cipherfinaloutputencoding) 方法来产生加密数据。

[crypto.createCipher()](./crypto.md#cryptocreatecipheralgorithm-password) 和 [crypto.createCipheriv()](./crypto.md#cryptocreatecipherivalgorithm-key-iv) 方法用于创建 `Cipher` 实例。`Cipher` 对象无法直接使用 `new` 关键词创建。

示例：将 `Cipher` 对象用作流：

``` javascript
const crypto = require('crypto');
const cipher = crypto.createCipher('aes192', 'a password');

var encrypted = '';
cipher.on('readable', () => {
    var data = cipher.read();
    if (data)
        encrypted += data.toString('hex');
});
cipher.on('end', () => {
    console.log(encrypted);
    // Prints: ca981be48e90867604588e75d04feabb63cc007a8f8ad89b10616ed84d815504
});

cipher.write('some clear text data');
cipher.end();
```

示例：使用 `Cipher` 并导流：

``` javascript
const crypto = require('crypto');
const fs = require('fs');
const cipher = crypto.createCipher('aes192', 'a password');

const input = fs.createReadStream('test.js');
const output = fs.createWriteStream('test.enc');

input.pipe(cipher).pipe(output);
```

示例：使用 [cipher.update()](#cipherupdatedata-inputencoding-outputencoding) 和 [cipher.final()](#cipherfinaloutputencoding) 方法：

``` javascript
const crypto = require('crypto');
const cipher = crypto.createCipher('aes192', 'a password');

var encrypted = cipher.update('some clear text data', 'utf8', 'hex');
encrypted += cipher.final('hex');
console.log(encrypted);
// Prints: ca981be48e90867604588e75d04feabb63cc007a8f8ad89b10616ed84d815504
```


## cipher.setAAD(buffer)

使用验证加密模式（目前仅支持 `GCM`）时，`cipher.setAAD()` 方法设置的值用于*附加认证数据*（AAD）输入参数。


## cipher.setAutoPadding(auto_padding=true)

当使用块加密算法时，`Cipher` 类会自动添加适当块大小的填充到输入数据中。调用 `cipher.setAutoPadding(false)` 禁用默认填充。

当 `auto_padding` 为 `false` 时，整个输入数据的长度必须是加密块大小的倍数，否则，[cipher.final()](#cipherfinaloutputencoding) 会抛出一个错误。禁用自动填充对于非标准填充有用，例如使用 `0x0` 而不是 PKCS 填充。

`cipher.setAutoPadding()` 方法必须在 [cipher.final()](#cipherfinaloutputencoding) 之前调用。


## cipher.getAuthTag()

使用验证加密模式（目前仅支持 `GCM`）时，`cipher.getAuthTag()` 方法返回一个包含已经从给定数据计算得出的*认证标签*的 [Buffer](../buffer/)。

`cipher.getAuthTag()` 方法只应在使用 [cipher.final()](#cipherfinaloutputencoding) 方法完成加密后才能调用。


## cipher.update(data[, input_encoding][, output_encoding])

用 `data` 更新加密内容。如果给定了 `input_encoding` 参数，它的值必须是 `'utf8'`、`'ascii'` 或 `'binary'` 其中之一并且 `data` 参数是使用指定编码的字符串。如果没有给定 `input_encoding` 参数，`data` 必须是一个 [Buffer](../buffer/)。如果 `data` 是一个 [Buffer](../buffer/)，那么 `input_encoding` 参数会被忽略。

`output_encoding` 指定加密数据的输出格式，可以是 `'binary'`、`'base64'` 或 `'hex'`。如果指定了 `output_encoding`，将返回使用指定编码的字符串。如果没有提供 `output_encoding`，将返回一个 [Buffer](../buffer/)。

`cipher.update()` 方法可以在新数据上多次调用，直到调用 [cipher.final()](#cipherfinaloutputencoding)。在 [cipher.final()](#cipherfinaloutputencoding) 之后调用 `cipher.update()` 将导致抛出错误。


## cipher.final([output_encoding])

返回任何剩余的加密内容。如果 `output_encoding` 参数是 `'binary'`、`'base64'` 或 `'hex'` 中的一个，将返回一个字符串。如果没有提供 `output_encoding`，将返回一个 [Buffer](../buffer/)。

一旦调用了 `cipher.final()` 方法，`Cipher` 对象不能再用于加密数据。尝试多次调用 `cipher.final()` 将导致抛出错误。