##### TypeArray

TypedArray 提供了对二进制数据 buffer 的各种整型类型“视图”

- Int8Array(8 位有符号整型)，Uint8Array(8 位无符号整型) ——Uint8ClampedArray(8 位无符号整型，每个值会被强制设置为在 0-255 内);
- Int16Array(16 位有符号整型), Uint16Array(16 位无符号整型);
- Int32Array(32 位有符号整型), Uint32Array(32 位无符号整型);
- Float32Array(32 位浮点数，IEEE-754);
- Float64Array(64 位浮点数，IEEE-754)。

##### Map

相比普通对象，键可以为任意值

```javascript
var m = new Map(),
  x = { id: 1 };

// 设置键值
m.set(x, 'foo');
// 获取对应键的值
m.get(x);
// 判断是否有某一个键
m.has(x);
// 删除对应键的值
m.delete(x);
// 清空整个Map
m.clear();
// 获取Map长度
m.size;
// 创建一个m的副本
var n = new Map(m); // 等价于 var n = new Map(m.entries());

===============


var x = { id: 1 },
  y = { id: 2 };
var m = new Map([[x, 'foo'], [y, 'bar']]);
m.get(x);
m.get(y);
[...m.keys()]  // [{id: 1}, {id: 2}]
[...m.values()] // ['foo', 'bar']
[...m.entries()] // [[{id: 1}, 'foo'],[{id: 2}, 'bar']]
```

WeakMap 是 map 的变体, 区别在于内部内存分配(特别是其 GC)的工作方式, 只接受对象作为键。这些对象是被弱持有的，也就是说如果对象本身被垃圾回收的话，在 WeakMap 中的这个项目也会被移除。

```javascript
var m = new WeakMap();
var x = { id: 1 },
  y = { id: 2 },
  z = { id: 3 },
  w = { id: 4 };
m.set(x, y);
x = null; // { id: 1 } 可GC
y = null; // { id: 2 } 可GC  // 只因 { id: 1 } 可GC
m.set(z, w);
w = null; // { id: 4 } 不可GC
```

##### Set

set 是一个值的集合，其中的值唯一(重复会被忽略)。
set 的 API 和 map 类似。只是 add(..) 方法代替了 set(..) 方法，没有 get(..) 方法。
WeakSet 的值必须是对象，而并不像 set 一样可以是原生类型值, 其中的值是弱持有的， 也就是说如果其中的项目是对这个对象最后一个引用的时候，GC 可以移除它.

```javascript
var s = new WeakSet();
var x = { id: 1 },
  y = { id: 2 };
s.add(x).add(y);
x = null; // x 可 GC
y = null; // y 可 GC
```
