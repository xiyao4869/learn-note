##### :peach: 回调函数缺点

- 代码逻辑书写顺序与执行顺序不一致，不利于阅读与维护。
- 异步操作的顺序变更时，需要大规模的代码重构。
- 回调函数基本都是匿名函数，bug 追踪困难。
- 回调函数是被第三方库代码（如 ajax ）而非自己的业务代码所调用的，造成了控制反转
  - 调用回调过早;
  - 调用回调过晚(或不被调用);
  - 调用回调次数过少或过多;
  - 未能传递所需的环境和参数;
  - 吞掉可能出现的错误和异常

callback 可能把回调函数交给第三方，由第三方来调用，存在信任问题，无法保证及时准确的调用。我们可以不把程序的下一步传给第三方，而是希望第三方给我们提供了解其任务何时结束的能力，然后由我们自己的代码来决定下一步做什么，这就是 promise。

---

##### :snowflake: setTimeout

setTimeout(..) 所做的是设定一个定时器, 当定时器到时后，环境会把你的回调函数放在事件循环中，这样，在未来某个时刻的 tick 会摘下并执行这个回调。
使用 setTimeout(..0) (hack)进行异步调度，就是“把这个函数插入到当前事件循环队列的结尾处”。

---

##### :cherries: Promise

resolve / reject: 作为 Promise 暴露给第三方库的 API 接口，在异步操作完成时由第三方库调用，从而改变 Promise 的状态。
fulfilled / rejected / pending : 标识了一个 Promise 当前的状态。
then : 作为 Promise 暴露给第一方代码的接口，在此传入【原本直接传给第三方库】的回调函数。

```javascript
// 将 ajax 请求封装为一个返回 Promise 的函数
function getData() {
  return new Promise((resolve, reject) => {
    ajax.get('xxx', data => {
      resolve(data);
    });
  });
}

// 调用该函数并在 Promise 的 then 接口中获取数据
getData().then(data => {
  console.log(data);
});
```

Promise.resolve(..) 返回值为 promise，如果传入的是 promise，则不做处理，直接返回它本身。
Promise.resolve(..) 可以为所有函数的返回值(不管是不是 thenable)都封装一层，比如 fn 函数，有时会返回一个立即值，有时会返回 Promise，那么 Promise.resolve( fn() )用来保证总会返回一个 Promise。

```javascript
function fn() {
  return 888; //立即值
}
// 不要只是这么做:
foo().then(function(v) {
  console.log(v);
});
// 而要这么做:
Promise.resolve(fn()).then(function(v) {
  console.log(v); //888
});
```

resolve(..) 和 Promise.resolve(..) 可以接受 promise 并接受它的状态 / 决议，而 reject (..) 和 Promise.reject(..) 并不区分接收的值是什么。所以，如果传入 promise 或 thenable 来拒绝， 这个 promise / thenable 本身会被设置为拒绝原因，而不是其底层值。

reject(..) 就是拒绝这个 promise; 但 resolve(..) 既可能完成 promise，也可能拒绝，要根据传入参数而定。如果传给 resolve(..) 的是一个非 Promise、非 thenable 的立即值，这个 promise 就会用这个值完成。
但是，如果传给 resolve(..) 的是一个真正的 Promise 或 thenable 值，这个值就会被递归展开，并且(要构造的)promise 将取用其最终决议值或状态。

then(..) 接受一个或两个参数:第一个用于完成回调，第二个用于拒绝回调。如果两者中的任何一个被省略或者作为非函数值传入的话，就会替换为相应的默认回调。默认完成回调只是把消息传递下去，而默认拒绝回调则只是重新抛出(传播)其接收到的出错原因。catch(..) 只接受一个拒绝回调作为参数，并自动替换默认完成回调。

Promise.all([ ]) 将会立即完成(没有完成值)，Promise.race([ ]) 将会永远挂起

```javascript
var rejectedPr = new Promise(function(resolve, reject) {
  // resolve(..)用于决议/完成这个promise
  // reject(..)用于拒绝这个promise
  // 用一个被拒绝的 promise 完成这个 promise
  resolve(Promise.reject('Oops'));
});
rejectedPr.then(
  function fulfilled() {
    // 永远不会到达这里
  },
  function rejected(err) {
    console.log(err); // "Oops"
  }
);
```

##### :strawberry: 手写 promise

[原文地址](https://segmentfault.com/a/1190000012664201#articleHeader1)

```javascript
// 判断变量否为function
const isFunction = variable => typeof variable === 'function';
// 定义Promise的三种状态常量
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

class MyPromise {
  constructor(handle) {
    if (!isFunction(handle)) {
      throw new Error('MyPromise must accept a function as a parameter');
    }
    // 添加状态
    this._status = PENDING;
    // 添加值
    this._value = undefined;
    // 添加成功回调函数队列
    this._fulfilledQueues = [];
    // 添加失败回调函数队列
    this._rejectedQueues = [];
    // 执行handle
    try {
      handle(this._resolve.bind(this), this._reject.bind(this));
    } catch (err) {
      this._reject(err);
    }
  }
  // 添加resovle时执行的函数
  _resolve(val) {
    const run = () => {
      if (this._status !== PENDING) return;
      // 依次执行成功队列中的函数，并清空队列
      const runFulfilled = value => {
        let cb;
        while ((cb = this._fulfilledQueues.shift())) {
          cb(value);
        }
      };
      // 依次执行失败队列中的函数，并清空队列
      const runRejected = error => {
        let cb;
        while ((cb = this._rejectedQueues.shift())) {
          cb(error);
        }
      };
      /* 如果resolve的参数为Promise对象，则必须等待该Promise对象状态改变后,
          当前Promsie的状态才会改变，且状态取决于参数Promsie对象的状态
        */
      if (val instanceof MyPromise) {
        val.then(
          value => {
            this._value = value;
            this._status = FULFILLED;
            runFulfilled(value);
          },
          err => {
            this._value = err;
            this._status = REJECTED;
            runRejected(err);
          }
        );
      } else {
        this._value = val;
        this._status = FULFILLED;
        runFulfilled(val);
      }
    };
    // 为了支持同步的Promise，这里采用异步调用
    setTimeout(run, 0);
  }
  // 添加reject时执行的函数
  _reject(err) {
    if (this._status !== PENDING) return;
    // 依次执行失败队列中的函数，并清空队列
    const run = () => {
      this._status = REJECTED;
      this._value = err;
      let cb;
      while ((cb = this._rejectedQueues.shift())) {
        cb(err);
      }
    };
    // 为了支持同步的Promise，这里采用异步调用
    setTimeout(run, 0);
  }
  // 添加then方法
  then(onFulfilled, onRejected) {
    const { _value, _status } = this;
    // 返回一个新的Promise对象
    return new MyPromise((onFulfilledNext, onRejectedNext) => {
      // 封装一个成功时执行的函数
      let fulfilled = value => {
        try {
          if (!isFunction(onFulfilled)) {
            onFulfilledNext(value);
          } else {
            let res = onFulfilled(value);
            if (res instanceof MyPromise) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFulfilledNext, onRejectedNext);
            } else {
              //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
              onFulfilledNext(res);
            }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err);
        }
      };
      // 封装一个失败时执行的函数
      let rejected = error => {
        try {
          if (!isFunction(onRejected)) {
            onRejectedNext(error);
          } else {
            let res = onRejected(error);
            if (res instanceof MyPromise) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFulfilledNext, onRejectedNext);
            } else {
              //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
              onFulfilledNext(res);
            }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err);
        }
      };
      switch (_status) {
        // 当状态为pending时，将then方法回调函数加入执行队列等待执行
        case PENDING:
          this._fulfilledQueues.push(fulfilled);
          this._rejectedQueues.push(rejected);
          break;
        // 当状态已经改变时，立即执行对应的回调函数
        case FULFILLED:
          fulfilled(_value);
          break;
        case REJECTED:
          rejected(_value);
          break;
      }
    });
  }
  // 添加catch方法
  catch(onRejected) {
    return this.then(undefined, onRejected);
  }
  // 添加静态resolve方法
  static resolve(value) {
    // 如果参数是MyPromise实例，直接返回这个实例
    if (value instanceof MyPromise) return value;
    return new MyPromise(resolve => resolve(value));
  }
  // 添加静态reject方法
  static reject(value) {
    return new MyPromise((resolve, reject) => reject(value));
  }
  // 添加静态all方法
  static all(list) {
    return new MyPromise((resolve, reject) => {
      /**
       * 返回值的集合
       */
      let values = [];
      let count = 0;
      for (let [i, p] of list.entries()) {
        // 数组参数如果不是MyPromise实例，先调用MyPromise.resolve
        this.resolve(p).then(
          res => {
            values[i] = res;
            count++;
            // 所有状态都变成fulfilled时返回的MyPromise状态就变成fulfilled
            if (count === list.length) resolve(values);
          },
          err => {
            // 有一个被rejected时返回的MyPromise状态就变成rejected
            reject(err);
          }
        );
      }
    });
  }
  // 添加静态race方法
  static race(list) {
    return new MyPromise((resolve, reject) => {
      for (let p of list) {
        // 只要有一个实例率先改变状态，新的MyPromise的状态就跟着改变
        this.resolve(p).then(
          res => {
            resolve(res);
          },
          err => {
            reject(err);
          }
        );
      }
    });
  }
  finally(cb) {
    return this.then(
      value => MyPromise.resolve(cb()).then(() => value),
      reason =>
        MyPromise.resolve(cb()).then(() => {
          throw reason;
        })
    );
  }
}
```
