:eyes:

- shape-outside 文字环绕图形效果
- regions 内容流布局，像报纸那样
- mix-blend-mode/background-blend-mode 混合模式
- position: sticky  固定导航栏
- overscroll-behavior 滚动边界和滚动链接
- 多列布局
  > {
      -webkit-column-break-inside: avoid;  /* Chrome, Safari, Opera */
      page-break-inside: avoid;    /* Firefox */
      break-inside: avoid;    /* IE 10+ */
  > }
- document.activeElement.scrollIntoView
- text-align-last // text-align:justify 对最后一行文本无效

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
