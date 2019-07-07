##### :strawberry: border
```js
1.使用透明border增加点击区域，相对于使用padding的优势在于，如果图标使用的是精灵图之类的，用padding可能会显示其他图标

2.右侧定位,默认 background 背景图片是相对于 padding box 定位的
.box {
  border-right: 50px solid transparent;
  background-position: 100% 50%;
}//距离右侧50px位置
``` 
##### :shaved_ice: margin
```js
1.只能使用子元素的 margin-bottom 来实现滚动容器的底部留白。

2.如果想让某个块状元素右对齐，脑子里不要就一个 float:right，很多时候，margin-left:auto 才是最佳的实践

3.利用margin: auto 实现垂直居中，触发 margin:auto 计算有一个前提条件，就是 width 或 height 为 auto 时， 元素是具有对应方向的自动填充特性的。
.father {
  width: 300px; height:150px;
  position: relative;
}
.son {
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
  width: 200px; height: 100px;
  margin: auto;
}
```

##### :lemon: line-height 与 vertical-align
```js
1.div高度是由行高决定的，而非文字。

2./* span换行实现 */
.test > span:first-child:after {
    content: "\A";
    white-space: pre;
}

3.图片为内联元素，会构成一个“行框盒子”，而在 HTML5 文档模式下，每一个“行框盒 子”的前面都有一个宽度为 0 的“幽灵空白节点”。

4.对于纯文 本元素，line-height 非常威风，直接决定了最终的高度。但是，如果同时有替换元素，则 line-height 的表现一下子弱了很多，只能决定最小高度
对于块级元素，line-height 对其本身是没有任何作用的，我们平时改变 line-height， 块级元素的高度跟着变化实际上是通过改变块级元素里面内联级别元素占据的高度实现的。

5.要让单行文字垂直居中，只需要 line-height 这一个属性就可以，与 height 一点儿关系都没有

6.vertical- align 属性值可为数值 如5px

7.在 CSS 世界中，凡是百分比值，均是需要一个相对计算的值，例如，margin 和 padding 是相对于宽度计算的，line-height 是相对于 font-size 计算的，而这里的 vertical- align 属性的百分比值则是相对于 line-height 的计算值计算的。

8.vertical-align 属性只能作用在 display 计算值为 inline、inline- block，inline-table 或 table-cell 的元素上。因此，默认情况下，<span>、<strong>、 <em>等内联元素，<img>、<button>、<input>等替换元素，非 HTML 规范的自定义标签 元素，以及<td>单元格，都是支持 vertical-align 属性的，其他块级元素则不支持。

9.vertical-align 属性的默认值 baseline 在文本之类的内联元素那里就是字符 x 的下边缘，对于替换元素则是替换元素的下边缘。但是，如果是 inline-block 元素，则规则要复杂了:一个 inline-block 元素，如果里面没有内联元素，或者 overflow 不是 visible， 则该元素的基线就是其 margin 底边缘;否则其基线就是元素里面最后一行内联元素的基线。

10.vertical-align:text-top:盒子的顶部和父级内容区域的顶部对齐。
vertical-align:text-bottom:盒子的底部和父级内容区域的底部对齐。
vertical-align:middle 定义是元素的中线和字符 x 中心点对齐。
虽然同属线性类属性值，但是 top/bottom 和 baseline/middle 却是完全不同的两个帮派，前者对齐看边缘看行框盒子，而后者是和字符 x 打交道。

11. 小图标与文字对齐场景，可设置图标高度为1ex，那么默认基线对齐，不需要再写vertical-align: middle
```