##### Array.of(..)

Array.of(..) 取代了 Array(..) 成为数组的推荐函数形式构造器.

```javascript
var a = Array(3);
a.length; // 3
a[0]; // undefined

var b = Array.of(3);
b.length; // 1
b[0]; // 3

var c = Array.of(1, 2, 3);
c.length; // 3
c; // [1,2,3]
```

##### Array.from(..)

Array.from(..) 接收一个可选的第三个参 数，如果设置了的话，这个参数为作为第二个参数传入的回调指定 this 绑定。否则，this 将会是 undefined。

```javascript
var arrLike = {
  length: 4,
  2: 'foo'
};
Array.from(
  arrLike,
  function mapper(val, idx) {
    console.log(this); // console.log({ a: 999 })
    if (typeof val == 'string') {
      return val.toUpperCase();
    } else {
      return idx;
    }
  },
  { a: 999 }
); // [ 0, 1, "FOO", 3 ]
```

##### copyWithin(..)

copyWithin(..) 从一个数组中复制一部分到同一个数组的另一个位置， 覆盖这个位置所有原来的值。

参数是 target(要复制到的索引)、start(开始复制的源索引，包括在内)以及可选的 end (复制结束的不包含索引)。如果任何一个参数是负数，就被当作是相对于数组结束的相对值。

```javascript
[1, 2, 3, 4, 5].copyWithin(3, 0);
[1, 2, 3, 4, 5].copyWithin(3, 0, 1);
[1, 2, 3, 4, 5].copyWithin(0, -2);
[1, 2, 3, 4, 5].copyWithin(0, -2, -1);
[1, 2, 3, 4, 5].copyWithin(2, 1);

// [1,2,3,1,2]
// [1,2,3,1,5]
// [4,5,3,4,5]
// [4,2,3,4,5]
// [1,2,2,3,4]
```

##### fill

fill(..) 可选地接收参数 start 和 end，它们指定了数组要填充的子集位置

```javascript
var a = [null, null, null, null].fill(42, 1, 3);

a; // [null,42,42,null]
```

##### find findIndex

find 可传入第二个参数绑定 this

```javascript
var a = [1, 2, 3, 4, 5];
a.find(function matcher(v) {
  return v == '2';
}); // 2
a.find(function matcher(v) {
  return v == 7;
}); // undefined
```

迭代器方法 entries()、values() 和 keys()

```javascript
var a = [1, 2, 3];
[...a.values()]; // [1,2,3]
[...a.keys()]; // [0,1,2]
[...a.entries()]; // [ [0,1], [1,2], [2,3] ]
[...a[Symbol.iterator]()]; // [1,2,3]
```

##### Object.is

你应该继续使用 === 进行严格相等比较;不应该把 Object.is(..) 当作这个运算符的替代。 但是，如果需要严格识别 NaN 或者 -0 值，那么应该选择 Object.is(..)

```javascript
var x = NaN,
  y = 0,
  z = -0;

x === x; // false
y === z; // true
Object.is(x, x); // true
Object.is(y, z); // false
```

##### Object.getOwnPropertySymbols(..)

从对象上取得所有的符号属性

```javascript
var o = {
  foo: 42,
  [Symbol('bar')]: 'hello world',
  baz: true
};
Object.getOwnPropertySymbols(o); // [ Symbol(bar) ]
```

##### Object.setPrototypeOf(..)

```javascript
var o1 = {
  foo() {
    console.log('foo');
  }
};
var o2 = {
  // .. o2的定义 ..
};
Object.setPrototypeOf(o2, o1);
// 委托给o1.foo()
o2.foo();
```

##### Object.assign(..)

不可枚举的属性和非自有的属性都被排除在赋值过程之外

##### Number

```javascript
Number.EPSILON; // 任意两个值之间的最小差, 浮点数算法的精度误差值

Number.MAX_SAFE_INTEGER;

Number.MIN_SAFE_INTEGER;

isNaN('abc'); // true

Number.isNaN('abc'); // false

// 标准的全局 isFinite(..) 会对参数进行强制类型转换，但是 Number.isFinite(..) 不会

var a = '42';
isFinite(a); // true
Number.isFinite(a); // false
```
