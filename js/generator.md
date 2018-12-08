##### :grapes: yield 与 next

```javascript
// yield.. 作为一个表达式可以发出消息响应 next(..) 调用，next(..) 也可以向暂停的 yield 表达式发送值
// yield表达式如果用在另一个表达式之中，必须放在圆括号里面
// next 方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的
function* foo(x) {
  var y = x * (yield 'Hello'); // <-- yield一个值!
  return y;
}
var it = foo(6);
var res = it.next();
res.value; // "Hello"
res = it.next(7); // 7为表达式(yield 'Hello')的返回值， 所以 var y = 6 * 7;
res.value; // 42
```

```javascript
function* foo() {
  var x = yield 2;
  z++;
  var y = yield x * z;
  console.log(x, y, z);
}
var z = 1;
var it1 = foo();
var it2 = foo();

var val1 = it1.next().value; // 2 <-- yield 2
var val2 = it2.next().value; // 2 <-- yield 2
val1 = it1.next(val2 * 10).value; // 40 <-- x:20, z:2    // val2 * 10 = 20 作为 yield 2 的返回值，即x = 20
val2 = it2.next(val1 * 5).value; // 600 <-- x:200, z:3
it1.next(val2 / 2); // 20 300 3
it2.next(val1 / 4); // 200 10 3
```

---

##### :tangerine: for...of 循环

```javascript
// 一旦 next 方法的返回对象的 done 属性为true，for...of循环就会中止，且不包含该返回对象
function* foo() {
  yield 1;
  yield 2;
  return 3;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2
```

---

##### :peach: 迭代器对象、可迭代对象、生成器

```javascript
// 迭代器对象： 有next方法, next方法返回一个对象，该对象拥有 done 和 value 两个属性
// 可迭代对象： 有[Symbol.iterator]方法，返回一个迭代器

// 调用生成器后返回的对象既有[Symbol.iterator]方法，又有next方法, [Symbol.iterator]方法 return 的是 this，即对象本身。
function* fn() {}
var f = fn();
f[Symbol.iterator]() === f; // true
```

---

##### :green_apple: generator 异步应用

```javascript
// generator 异步应用
function foo(x, y) {
  ajax('http://some.url.1/?x=' + x + '&y=' + y, function(err, data) {
    if (err) {
      // 向*main()抛出一个错误
      it.throw(err);
    } else {
      // 用收到的data恢复*main()
      it.next(data);
    }
  });
}
function* main() {
  try {
    var text = yield foo(11, 31); // text 的值为传入的 data
    console.log(text);
  } catch (err) {
    console.error(err);
  }
}
var it = main();
// 这里启动!
it.next();
```

```javascript
// promise + generator = async await
// yield 一个promise， 等 promise 决议之后把值返回给 yield 表达式
function foo() {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve('foo');
    }, 5000);
  });
}
function run(gen) {
  var args = [].slice.call(arguments, 1),
    it;
  // 在当前上下文中初始化生成器
  it = gen.apply(this, args);
  // 返回一个promise用于生成器完成
  return Promise.resolve().then(function handleNext(value) {
    // 对下一个yield出的值运行
    var next = it.next(value);
    return (function handleResult(next) {
      // 生成器运行完毕了吗?
      if (next.done) {
        return next.value;
      }
      // 否则继续运行
      else {
        return Promise.resolve(next.value).then(
          // 成功就恢复异步循环，把决议的值发回生成器 handleNext,
          handleNext,
          // 如果value是被拒绝的 promise，
          // 就把错误传回生成器进行出错处理
          function handleErr(err) {
            return Promise.resolve(it.throw(err)).then(handleResult);
          }
        );
      }
    })(next);
  });
}
function* main() {
  try {
    var text = yield foo(11, 31);
    console.log(text);
  } catch (err) {
    console.error(err);
  }
}
run(main);
```

---

##### :corn: es6 之前生成器实现

```javascript
function* foo(url) {
  // 状态1
  try {
    console.log('requesting:', url);
    var TMP1 = request(url);
    // 状态2
    var val = yield TMP1;
    console.log(val);
  } catch (err) {
    // 状态3
    console.log('Oops:', err);
    return false;
  }
}

// 生成器es6之前的实现  我的理解：生成器是一个状态机
function foo(url) {
  // 管理生成器状态
  var state;
  // 生成器变量范围声明
  var val;
  function process(v) {
    switch (state) {
      case 1:
        console.log('requesting:', url);
        return request(url);
      case 2:
        val = v;
        console.log(val);
        return;
      case 3:
        var err = v;
        console.log('Oops:', err);
        return false;
    }
  }
  return {
    next: function(v) {
      // 初始状态
      if (!state) {
        state = 1;
        return {
          done: false,
          value: process()
        };
      }
      // yield成功恢复
      else if (state == 1) {
        state = 2;
        return {
          done: true,
          value: process(v)
        };
      }
      // 生成器已经完成
      else {
        return {
          done: true,
          value: undefined
        };
      }
    },
    throw: function(e) {
      // 唯一的显式错误处理在状态1
      if (state == 1) {
        state = 3;
        return {
          done: true,
          value: process(e)
        };
      }
      // 否则错误就不会处理，所以只把它抛回
      else {
        throw e;
      }
    }
  };
}

(1) 对迭代器的 next() 的第一个调用会把生成器从未初始化状态转移到状态 1 ，然后调用 process() 来处理这个状
态。request(..) 的返回值是对应 Ajax 响应的 promise，作为 value 属性从 next() 调用返回。

(2) 如果 Ajax 请求成功，第二个 next(..) 调用应该发送 Ajax 响应值进来，这会把状态转移到状态 2 。再次调用
process(..) (这次包括传入的 Ajax 响应值)，从 next(..) 返回的 value 属性将是 undefined 。

(3) 然而，如果 Ajax 请求失败的话，就会使用错误调用 throw(..) ，这会把状态从 1 转移到 3 (而非 2 )。再次调用
process(..) ，这一次包含错误值。这个 case 返回 false ，被作为 throw(..) 调用返回的 value 属性。

```

---

##### :cherries: thunk

```javascript
function foo(x, y, cb) {
  setTimeout(function() {
    cb(x + y);
  }, 1000);
}

function thunkify(fn) {
  return function() {
    var args = [].slice.call(arguments);
    return function(cb) {
      args.push(cb);
      return fn.apply(null, args);
    };
  };
}

var fooThunkory = thunkify(foo);
var fooThunk1 = fooThunkory(3, 4);
fooThunk1(function(sum) {
  console.log(sum); // 7
});
```

---

##### :shaved_ice: 练习

```javascript
// 默认的数组迭代器并不关心通过 next(..) 调用发送的任何消息，所以值 2 、3 和 4 根本就被忽略了。还有，因为迭代器没有显式的返回值(和前面使用的*foo()不同)，所以yield *表达式完成后得到的是一个undefined
function* bar() {
  console.log('inside *bar():', yield 'A');
  // yield委托给非生成器!
  console.log('inside *bar():', yield* ['B', 'C', 'D']);
  console.log('inside *bar():', yield 'E');
  return 'F';
}
var it = bar();
console.log('outside:', it.next().value); // outside: A
console.log('outside:', it.next(1).value);
// inside *bar(): 1
// outside: B
console.log('outside:', it.next(2).value); // outside: C
console.log('outside:', it.next(3).value); // outside: D
console.log('outside:', it.next(4).value);
// inside *bar(): undefined
// outside: E
console.log('outside:', it.next(5).value); // inside *bar(): 5
// outside: F
```

```javascript
function* foo() {
  console.log('inside *foo():', yield 'B');
  console.log('inside *foo():', yield 'C');
  return 'D';
}
function* bar() {
  console.log('inside *bar():', yield 'A');
  // yield 委托!
  console.log('inside *bar():', yield* foo());
  console.log('inside *bar():', yield 'E');
  return 'F';
}
var it = bar();
console.log('outside:', it.next().value); // outside: A
console.log('outside:', it.next(1).value);
// inside *bar(): 1
// outside: B

console.log('outside:', it.next(2).value);
// inside *foo(): 2
// outside: C

console.log('outside:', it.next(3).value);
// inside *foo(): 3
// inside *bar(): D
// outside: E

console.log('outside:', it.next(4).value);
// inside *bar(): 4
// outside: F
```
