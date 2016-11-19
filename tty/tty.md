# 方法和属性

* [tty.isatty(fd)](#ttyisattyfd)
* [tty.setRawMode(mode)](#ttysetrawmodemode)

--------------------------------------------------

## tty.isatty(fd)

返回 `true` 或 `false` 取决于 `fd` 是否与终端相关联。


## tty.setRawMode(mode)

> 稳定度：0 - 已废弃：使用 [tty.ReadStream#setRawMode](./class_ReadStream.md#rssetrawmodemode)（即 process.stdin.setRawMode）代替。