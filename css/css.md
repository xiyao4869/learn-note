##### border
```js
1.使用透明border增加点击区域，相对于使用padding的优势在于，如果图标使用的是精灵图之类的，用padding可能会显示其他图标
2.右侧定位,默认 background 背景图片是相对于 padding box 定位的
.box {
  border-right: 50px solid transparent;
  background-position: 100% 50%;
}//距离右侧50px位置
``` 