```javascript
function foo(..) {
  // ..
}
// 导出的是此时到函数表达式值的绑定，而不是标识符 foo。export default ..接受的是一个表达式。如果之后给foo赋一个不同的值，模块导入得到的仍然是原来导出的函数，而不是新的值
export default foo;
foo = null;
import 导入时不会报错


export default function foo(..){}  //不是表达式 导出名字default
foo = null;
import 导入时值为null 会报错


function foo(..) {
  // ..
}
// 默认导出绑定实际上绑定到 foo 标识符而不是它的值，如果之后修改了 foo 的值，在导入一侧看到的值也会更新。
export { foo as default };
foo = null;
import 导入时会报错
```

```javascript
// 默认导出时的导入方式
import foo from 'foo';
// 或者:
import { default as foo } from 'foo';
```

```javascript
export var bar = "hello world"; // 不能用export default

========

var bar = "hello world";
export default bar;
```
