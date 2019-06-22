1.ios日期转换问题
编写日期组件时，当使用new Date("2019-5-20 8:00")获取日期时, Safari浏览器会报错，出现NaN，采用new Date("2019/5/20 8:00")的方式可兼容

2.安卓手机部分版本input里的placeholder位置偏上
input不要设line-height 或 line-height:normal;

3.小程序点击文字全屏播放视频问题
利用display:none隐藏video标签来播放时，苹果可以正常播放，安卓无效，替换方式：采用height：0来隐藏video标签

4.ios下fixed失效
场景：聊天页面，底部有固定输入框
点击输入框，唤醒键盘后，当页面的内容超过一屏且滚动时，失效的fixed元素（底部输入框）会跟随滚动，可修改页面滚动为容器内滚动，另外给滚动容器添加-webkit-overflow-scrolling: touch;保持滚动流畅度。

5.移动端滚动穿透
当滚动页面上弹出框的内容时，底下页面也会跟随滚动，可在弹框出现时给body添加position:fixed，关闭弹框后去掉fixed, 并且弹出框时记录滚动条的位置，在关闭弹框时恢复滚动条位置( 新属性overscroll-behavior也可阻止滚动，但兼容性不够好)

6.iPhoneX适配
meta 属性中 添加 viewport-fit=cover
利用 safe-area-inset-top/safe-area-inset-right/safe-area-inset-bottom/safe-area-inset-left 设置对应padding， 使内容显示在安全区域内

7.移动端点击输入框弹出自定义键盘
原生input会弹出系统的软键盘，需要div元素模拟输入框，设置div的contentEditable='true',如果需要聚焦可使用tabindex属性聚焦



移动端fixed元素+input
业务场景：聊天页面，底部有固定输入框
技术问题：在ios系统下，当软键盘唤起后，页面的fixed元素会失效，当页面内容超过一屏并且滚动的时候，失效的fixed元素会跟随页面滚动起来
解决方案一：
1.考虑到fixed元素是在页面超过一屏内容并且滚动时才出现的现象，可以把原先的页面滚动修改为容器内滚动, 页面结构大致如下（根据实际情况调整）：<main><div class="content">页面主要内容</div></main><footer><input/></footer>
2.即修改主容器main的高度为calc(100vh - fixed元素的高度), 并设置overflow: auto; 
3.为保持在真机上的滚动流畅性，给main添加-webkit-overflow-scrolling: touch;

解决方案二：
1.既然是fixed失效导致的现象，可以考虑不使用fixed
2.可设置页面高度为100vh，然后采用flex布局，main设置flex:1, 底部元素也不需要fixed定位了，相比方案一也可以免于计算main的高度
3.给main设置overflow: auto;和-webkit-overflow-scrolling: touch;

此外，有时点击输入框弹出软键盘后，部分iOS版本会出现输入框被遮挡现象，可先进行版本判断，然后监听输入框的聚焦，使用inputElement.scrollIntoView的方式尝试解决，如果用户采用第三方输入法，因为第三方输入法比原生的多个toolbar，也会出现遮挡，可考虑设计上规避，转场输入等方式。

