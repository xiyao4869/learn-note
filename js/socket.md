[心跳重连] https://juejin.im/post/5b5ad3c16fb9a04fab451c9d

```javascript
// 每隔10ms向服务器发送消息 若服务器有回复 执行reset方法并重新开始start 如果在10ms内没有收到回复 会调用wx.closeSocket方法关闭连接 然后会触发onSocketClose进行重新连接
let heartCheck = {
  timeout: 10000,
  timeoutObj: null,
  serverTimeoutObj: null,
  reset: function() {
    clearTimeout(this.timeoutObj);
    clearTimeout(this.serverTimeoutObj);
    return this;
  },
  start: function() {
    this.timeoutObj = setTimeout(() => {
      console.log('发送ping');
      wx.sendSocketMessage({
        data: 'ping'
        // success(){
        //   console.log("发送ping成功");
        // }
      });
      this.serverTimeoutObj = setTimeout(() => {
        wx.closeSocket();
      }, this.timeout);
    }, this.timeout);
  }
};

wx.onSocketMessage(res => {
  //收到服务器消息
  if (res.data == 'pong') {
    heartCheck.reset().start();
  } else {
  }
});
wx.onSocketOpen(() => {
  console.log('WebSocket连接打开');
  heartCheck.reset().start();
});
wx.onSocketError(res => {
  console.log('WebSocket连接打开失败');
  this.reconnect();
});
wx.onSocketClose(res => {
  console.log('WebSocket 已关闭！');
  this.reconnect();
});
```
