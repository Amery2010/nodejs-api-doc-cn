# socket.bind() 行为变为异步

截至 Node.js v0.10，[dgram.Socket#bind()](./class_dgram_Socket.md#socketbindoptions-callback) 更改为异步执行模式。遗留代码假定为同步行为，如以下示例所示：

``` javascript
const s = dgram.createSocket('udp4');
s.bind(1234);
s.addMembership('224.0.0.114');
```

必须改成传递一个回调函数到 [dgram.Socket#bind()](./class_dgram_Socket.md#socketbindoptions-callback) 函数：

``` javascript
const s = dgram.createSocket('udp4');
s.bind(1234, () => {
    s.addMembership('224.0.0.114');
});
```