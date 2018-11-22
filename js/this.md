- this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式
- 默认绑定:
  > 虽然 this 的绑定规则完全取决于调用位置，但是只 有 foo() 运行在非 strict mode 下时，默认绑定才能绑定到全局对象;严格模式下与 foo() 的调用位置无关

```javascript
function foo() {
  'use strict';
  console.log(this.a);
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
  foo(); // 2
})();
```
