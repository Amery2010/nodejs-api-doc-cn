# WriteStream类

* ['resize' 事件](#resize-事件)
* [ws.columns](#wscolumns)
* [ws.rows](#wsrows)

--------------------------------------------------

一个 `net.Socket` 子类，它表示 tty 的可写部分。在正常情况下，`process.stdout` 将是任何 Node.js 程序中唯一的 `tty.WriteStream` 实例（仅当 `isatty(1)` 为 true 时）。


## 'resize' 事件

`function () {}`

当 `columns` 或 `rows` 属性中的任何一个已更改时，由 `refreshSize()` 发出。

``` javascript
process.stdout.on('resize', () => {
    console.log('screen size has changed!');
    console.log(`${process.stdout.columns}x${process.stdout.rows}`);
});
```


## ws.columns

一个 `Number`，给出 TTY 当前具有的列数。此属性将更新 `'resize'` 事件。


## ws.rows

一个 `Number`，给出 TTY 当前具有的行数。此属性将更新 `'resize'` 事件。