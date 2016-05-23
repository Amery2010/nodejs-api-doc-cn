# maxBuffer 和 Unicode

`maxBuffer` 选项是在 `stdout` 或 `stderr` 上用于指定允许的最大*八位字节*数，记住这一点非常重要 —— 如果超过这个值，子进程将会被终止。这尤其影响包含多字节字符编码的输出，如 UTF-8 或 UTF-16。例如，以下将输出 13 个 UTF-8 编码的八位字节到 stdout，尽管只有 4 个字符：

```javascript
console.log('中文测试');
```