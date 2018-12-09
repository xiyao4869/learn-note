##### worker

[ Web Worker 使用 ](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)

- 这是浏览器(即宿主环境)的功能，实际上和 JavaScript 语言本身几乎没什么关系
- Worker 之间以及它们和主程序之间，不会共享任何作用域或资源，而是通过一个基本的事件消息机制相互联系
- 在 Worker 内部是无法访问主程序的任何资源的。这意味着你不能访问它的任何全局变量，也不能访问页面的 DOM 或者其他资源。记住，这是一个完全独立的线程。
- 可以执行网络操 作(Ajax、WebSockets)以及设定定时 器。还 有，Worker 可以访问几个重要的全局变量和功能的本地复本，包括 navigator 、location 、JSON 和 applicationCache 。还可以通过 importScripts(..) 向 Worker 加载额外的 JavaScript 脚本。
- 主线程与子线程数据通信方式有多种，通信内容，可以是文本，也可以是对象。需要注意的是，这种通信是拷贝关系，即是传值而不是地址，子线程对通信内容的修改，不会影响到主线程。事实上，浏览器内部的运行机制是，先将通信内容串行化，然后把串行化后的字符串发给子线程，后者再将它还原

```javascript
//主线程新建worker
var worker = new Worker('work.js'); // 由于 Worker 不能读取本地文件，所以这个脚本必须来自网络

// 主线程调用worker.postMessage()方法，向 Worker 发消息
worker.postMessage('Hello World');

// 主线程通过worker.onmessage指定监听函数，接收子线程发回来的消息
worker.onmessage = function(event) {
  console.log('Received message ' + event.data);
};

// 监听错误
worker.onerror(function(e) {
  console.log(e);
});

// Worker 完成任务以后，主线程就可以把它关掉。
worker.terminate();
```

---

##### Worker 线程 (work.js)

```javascript
// Worker 线程内部需要有一个监听函数，监听message事件, self代表子线程自身，即子线程的全局对象。也可以使用self.onmessage.
self.addEventListener(
  'message',
  function(e) {
    self.postMessage('You said: ' + e.data);
  },
  false
);

// 用于在 Worker 内部关闭自身
self.close();
```

##### Worker 加载脚本

```javascript
// 脚本加载是同步的, importScripts(..) 调用会阻塞余下 Worker 的执行，直到文件加载和执行完成
importScripts('foo.js', 'bar.js');
```
