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

```javascript
// 迭代器对象： 有next方法, next方法返回一个对象，该对象拥有 done 和 value 两个属性
// 可迭代对象： 有[Symbol.iterator]方法，返回一个迭代器

// 调用生成器后返回的对象既有[Symbol.iterator]方法，又有next方法, [Symbol.iterator]方法 return 的是 this，即对象本身。
function* fn() {}
var f = fn();
f[Symbol.iterator]() === f; // true
```

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
