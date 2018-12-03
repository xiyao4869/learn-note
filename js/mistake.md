```javascript
// 为null时解构赋默认值不生效， undefined才生效
var a = { b: null };
let { b: c = {} } = a;
c; // null
```

---

```javascript
// 按字母顺序比较
'12:00' < '9:30'; // true
'12:00' > '09:30'; // true
```

---

```javascript
// 严格模式会报错 b is not defined, 非严格模式会自动创建一个全局变量b
'use strict';
var a = (b = 45);
```

---

```javascript
[] + {}; // "[object Object]"  []强制转换为"" {}强制转换为"[object Object]"
{
}
+[]; // 0  {}解析为空代码块  +[]进行强制类型转换
```
