# Interface 类

* [rl.write(data[, key])](#rlwritedatakey)
* [rl.setPrompt(prompt)](#rlsetpromptprompt)
* [rl.prompt([preserveCursor])](#rlpromptpreservecursor)
* [rl.question(query, callback)](#rlquestionquery-callback)
* [rl.pause()](#rlpause)
* [rl.resume()](#rlresume)
* [rl.close()](#rlclose)

--------------------------------------------------

表示具有输入和输出流的 readline 的接口类。


## rl.write(data[, key])

向 `output` 流写入 `data`，除非在调用 `createInterface` 时，`output` 被设置为 `null` 或 `undefined`。`key` 是一个对象字面量表示的键序列；如果终端是 TTY，则可用。

这也会恢复 `input` 流，如果它已被暂停。

例子：

``` javascript
rl.write('Delete me!');
// Simulate ctrl+u to delete the line written previously
rl.write(null, {ctrl: true, name: 'u'});
```


## rl.setPrompt(prompt)

设置提示，例如，当你在命令行中运行 `node` 时，你会看到 `> `，这就是 Node.js 的提示。


## rl.prompt([preserveCursor])

为用户的输入准备 readline，将当前的 `setPrompt` 选项放在一个新行上，给用户一个新的写点。设置 `preserveCursor` 为 `true`，防止将光标位置重置为 `0`。

这也会恢复用于 `createInterface` 的 `input` 流，如果它已被暂停。

当调用 `createInterface` 时，如果 `output` 被设置为 `null` 或 `undefined`，该提示不会写入。


## rl.question(query, callback)

在提示符前面加上 `query` 并带着用户响应调用 `callback`。向用户显示查询，然后在用户输入后，带着用户响应调用 `callback`。

这也会恢复用于 `createInterface` 的 `input` 流，如果它已被暂停。

当调用 `createInterface` 时，如果 `output` 被设置为 `null` 或 `undefined`，将不会显示。

用法示例：

``` javascript
rl.question('What is your favorite food?', (answer) => {
    console.log(`Oh, so your favorite food is ${answer}`);
});
```


## rl.pause()

暂停 readline 的 `input` 流，如果需要，可以在之后恢复。

请注意，这不会立即暂停事件流。在调用 `pause` 之后，可能会发出几个事件，包括 `line`。


## rl.resume()

恢复 readline 的 `input` 流。


## rl.close()

关闭 `Interface` 实例，放弃控制 `input` 和 `output` 流。也同样会发出 `'close'` 事件。