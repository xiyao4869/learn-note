##### Virtual Dom 算法的实现

1. 通过 JS 来模拟创建 DOM 对象
2. 判断两个对象的差异
3. 渲染差异

##### render props vs HOC

https://reactjs.org/docs/render-props.html#use-render-props-for-cross-cutting-concerns

##### context

https://reactjs.org/docs/context.html#when-to-use-context

##### React 中 setState 什么时候是同步的，什么时候是异步的？

https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/17

```js
在 React 中，如果是由 React 引发的事件处理（比如通过 onClick 引发的事件处理），调用 setState 不会同步更新 this.state，除此之外的 setState 调用会同步执行 this.state。所谓“除此之外”，指的是绕过 React 通过 addEventListener 直接添加的事件处理函数，还有通过 setTimeout/setInterval 产生的异步调用。

在 React 的 setState 函数实现中，会根据一个变量 isBatchingUpdates 判断是直接更新 this.state 还是放到队列中回头再说，而 isBatchingUpdates 默认是 false，也就表示 setState 会同步更新 this.state，但是，有一个函数 batchedUpdates，这个函数会把 isBatchingUpdates 修改为 true，而当 React 在调用事件处理函数之前就会调用这个 batchedUpdates，造成的后果就是由 React 控制的事件处理过程 setState 不会同步更新 this.state。

isBatchingUpdates 为true，批量更新state

```
