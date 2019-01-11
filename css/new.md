:eyes:

- shape-outside 文字环绕图形效果
- regions 内容流布局，像报纸那样
- mix-blend-mode/background-blend-mode 混合模式
- position: sticky  固定导航栏
- overscroll-behavior 滚动边界和滚动链接
- scroll-behavior:smooth 平滑滚动
- 多列布局
  > {
      -webkit-column-break-inside: avoid;  /* Chrome, Safari, Opera */
      page-break-inside: avoid;    /* Firefox */
      break-inside: avoid;    /* IE 10+ */ 避免在元素内部插入分页符
  > }
- document.activeElement.scrollIntoView
- text-align-last // text-align:justify 对最后一行文本无效
- box-decoration-break: clone; // 指定元素片段在跨行、跨列或跨页（如打印）时候的样式渲染表现
- scroll-snap-type/scroll-snap-align //网页容器滚动停止的时候，自动平滑定位到指定元素的指定位置
- color-adjust:economy // 是否允许浏览器自己调节颜色

##### Element.scrollIntoView VS Element.scrollIntoViewIfNeeded

- Element.scrollIntoView()，无论元素是否在可视区域，均发生滚动。
- Element.scrollIntoViewIfNeeded(), 当元素已经在可视区域时，不发生滚动。
- Element.scrollIntoViewIfNeeded()在移动端的支持比 Element.scrollIntoView()好

##### pointer-events

- pointer-events: none; 元素永远不会成为鼠标事件的 target。但是，当其后代元素的 pointer-events 属性指定其他值时，鼠标事件可以指向后代元素，

##### 取消自动填充密码

```javascript
<form action="?" method="post">
    <input type="text" style="display: none;">
    <input type="password" class="form-control"  placeholder=""  autocomplete="new-password">
    <input type="submit" value="提交">
</form>
```

##### scss

scss 写法: http://www.ruanyifeng.com/blog/2012/06/sass.html

##### 占位图

http://dummyimage.com/120x120
http://img.la/190x80
