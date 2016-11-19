# ReadStream类

* [rs.isRaw](#rsisraw)
* [rs.setRawMode(mode)](#rssetrawmodemode)

--------------------------------------------------

一个 `net.Socket` 子类，它表示 tty 的可读部分。在正常情况下，`process.stdin` 将是任何 Node.js 程序中唯一的 `tty.ReadStream` 实例（仅当 `isatty(0)` 为 true 时）。


## rs.isRaw

一个 `Boolean`，初始化为 `false`。它表示 `tty.ReadStream` 实例的当前“原始”状态。


## rs.setRawMode(mode)

`mode` 应该是 `true` 或 `false`。这将设置 `tty.ReadStream` 的属性作为原始设备或默认值。`isRaw` 将被设置为结果模式。