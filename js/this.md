##### :snowflake: 判断 this

> 1.  由 new 调用?绑定到新创建的对象。 :arrow_down:

```javascript
var bar = new foo();
```

> 2.  由 call 或者 apply(或者 bind)调用?绑定到指定的对象。 :arrow_down:

```javascript
var bar = foo.call(obj2);
```

> 3.  由上下文对象调用?绑定到那个上下文对象。 :arrow_down:

```javascript
var bar = obj1.foo();
```

> 4.  默认:在严格模式下绑定到 undefined，否则绑定到全局对象。 :arrow_down:

```javascript
var bar = foo();
```

- this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式
- 默认绑定:

  > 对于默认绑定来说，决定 this 绑定对象的并不是调用位置是否处于严格模式，而是函数体是否处于严格模式。如果函数体处于严格模式，this 会被绑定到 undefined，否则 this 会被绑定到全局对象

  ```javascript
  function foo() {
    'use strict'; //函数体启用严格模式
    console.log(this.a); //this值为undefined
  }
  var a = 2;
  foo();
  ```

  ```javascript
  function foo() {
    console.log(this.a);
  }
  var a = 2;
  (function() {
    'use strict';
    foo(); // 2  与调用位置启用严格模式无关
  })();
  ```

- 间接引用(赋值)会应用默认绑定规则

```javascript
function foo() {
  console.log(this.a);
}
var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };
var m = { a: 5 };
o.foo(); // 3
m.foo = o.foo;
m.foo(); // 5
(p.foo = o.foo)(); // 2   p.foo = o.foo 返回值是目标函数的引用
```

- 箭头函数根据外层(函数或者全局)作用域来决定 this

- 忽略 this 绑定

```javascript
var ø = Object.create(null);
foo.apply(ø, [2, 3]); //替代 foo.apply(null, [2, 3]); 写法，保护全局作用域
```
