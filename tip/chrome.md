[来源] https://juejin.im/post/5c16d943518825566d2365f3

##### \$0

在 Chrome 的 Elements 面板中, \$0 是当前我们选中的 html 节点的引用。$1 是我们上一次选择的节点的引用, 一直到\$4

##### \$ 和 \$\$

```javascript
$等同 document.querySelector
$$()等同 Array.from(document.querySelectorAll())
$_变量是上次执行的结果的引用
```

##### monitor

```javascript
monitorEvents(\$0,'click');
monitor(function)
```

##### console 添加样式

```javascript
console.log('%cxiyao', 'color:red; font-size: 24px;');
```

##### 快捷键

```javascript
h  Element隐藏元素
⌘ + shift + D 切换开发工具位置
⌘ + shift + p 调出 command 面板 => Capture full size screenshot
Esc 打开/隐藏 drawer
```

##### 条件断点

```javascript
// 可在线上添加打印console信息
右击行号并且选择 Add conditional breakpoint...(添加条件断) 点的选项
或者右击一个已经设置的断点并且选择 Edit breakpoint(编辑断点)
```
