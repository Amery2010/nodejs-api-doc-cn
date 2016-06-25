# --zero-fill-buffers命令行选项

Node.js 可以在一开始就使用 `--zero-fill-buffers` 命令行选项强制所有新配置的 `Buffer` 和 `SlowBuffer` 实例，无论是通过 `new Buffer(size)` 还是 `new SlowBuffer(size)` ，都在创建时*自动用零填充*。使用这个标志*改变这些方法的默认行为*会对性能*有显著的影响*。建议只在有绝对必要新分配的 Buffer 实例不能包含潜在的敏感数据时才去执行 `--zero-fill-buffers` 选项。

```
$ node --zero-fill-buffers
> Buffer(5);
<Buffer 00 00 00 00 00>
```