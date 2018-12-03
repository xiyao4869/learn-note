```javascript
// 标签为bar的代码块
function foo() {
  bar: {
    console.log('Hello');
    break bar;
    console.log('never runs');
  }
  console.log('World');
}
foo(); // Hello  World
```

---

```javascript
// 向函数传递参数时，arguments 数组中的对应单元会和命名参数建立关联(linkage)以得到相同的值。相反，不传递参数就不会建立关联。在严格模式中没有建立关联。
function foo(a) {
  a = 42;
  console.log(arguments[0]);
}
foo(2); // 42 (linked)
foo(); // undefined (not linked)
```

```javascript
// 找不到匹配10的case 执行default 没有break  继续往下执行 直到break
var a = 10;
switch (a) {
  case 1:
  case 2:
    console.log('12');
  case 3:
    console.log('3');
  default:
    console.log('default');
  case 4:
    console.log('4');
  case 5:
    console.log('5');
    break;
  case 6:
    console.log('6');
}
// default
// 4
// 5
```

---

```javascript
// getter setter
var c = {
  _name: 'lucy',
  get fn() {
    console.log(this._name);
  },
  set fn(name) {
    this._name = name;
  }
};
c.fn; // lucy
c.fn = 'xiyao';
c.fn; // xiyao
```
