---
title: 无线端浏览器click事件300ms延迟
---

本文以[http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay][1]为主体, 结合其他资料构成

# 起因

2007年苹果为了解决iPhone Safari访问PC页面的缩放问题, 提供了一个**两次触摸触发缩放**功能. 当用户第一次触摸屏幕的时候
浏览器等待300ms才能判断用户是要点击(click)还是缩放(zoom), 这就造成用户触摸屏幕到click事件触发存在300ms延迟.

触摸屏幕事件流程如下:

1. touchstart
2. touchend
3. Wait 300ms in case of another tap
4. click

受到这个延迟影响的场景有:

- JavaScript监听的click事件
- 基于click的交互的元素, 例如链接, 表单元素

随着iPhone的成功, 后续的无线浏览器复制了它的大部分操作习惯, 其中就包括两次触摸触发缩放, 这也成为主流浏览器的一个功能.

这300ms延迟在平时浏览网页不会带来严重问题, 但是高性能web app使用时就是一个严重的问题, 响应式设计的流行也让双击缩放逐渐失去用武之地.

[点击本页面测试click延迟][6]

# 浏览器解决方案

除了Apple外的各大浏览器厂商都想要消除300ms延迟. 以下是一些解决方案


## 1. 取消禁止缩放页面的延迟

两次触摸是为了缩放页面, 在不能缩放的页面禁止延迟也就理所当然, 只需要设置以下`<meta>`标签即可

```
<meta name="viewport" content="width=device-width, user-scalable=no">
```

[安卓版chrome][3]首先引入这一特性, [安卓版Firefox][4]随后也实现了, 其他浏览器厂商没有这一实现的申明.

这个解决方案简单有效但是禁止缩放极大降低了页面的可使用性和可访问性, 查看小文本和图片会很困难. web游戏成为这个方案
最大的受益者.

## 2. 取消为mobile优化页面的延迟

```
<meta name="viewport" content="width=device-width">
```

Chrome团队宣布从Chrome 32开始[取消viewport设置为width小于等于device-width页面两次敲击缩放功能][5], 这样就消除300ms延迟.

因为设置了**width=device-width**的页面基本上都是响应式页面, 也就不需要缩放了. 确实需要缩放的时候还可以使用**pinch进行缩放**.

Firefox暂时没有任何表示是否采纳这个方案.

两次敲击不可缩放页面在iOS上是滚动手势, 如果要借鉴这个方案那必然会损失这个功能, 如果在不可缩放页面都不能消除延迟, 那可缩放页面消除延迟的可能性也不大. iOS将如何选择只能等时间告诉我们答案了.


不可缩放页面在windows phone上依然存在300ms延迟, 但是他们没有像iOS那样为两次敲击定义行为, 所以他们有可能采用这个方案.


## 3. 指针事件(Pointer Events)

微软提出一套指针事件, 试图用一个事件模型统一描述所有输入类型(鼠标, 触屏, 笔触). 这样只需要监听`pointerdown`而不需要
分别监听`touchstart`, `mousedown`, 现在已经成为[candidate recommendation W3C specification][7], 不过目前[只有IE11+支持][8]

其中新引入的CSS属性`touch-action`设置元素可以提供给用户的操作. 取值和含义如下:

- auto(默认): 有用户代理决定如何响应用户的行为(如滚动, 缩放等)
- none: 对用户行为不触发任何反应
- pan-x: 用户代理认为用户的触屏操作目的是水平滚动元素最近可水平滚动的祖先元素
- pan-y: 用户代理认为用户的触屏操作目的是竖直滚动元素最近可竖直滚动的祖先元素
- manipulation: 用户代理认为用户触屏操作只是为了滚动和连续缩放. `auto`所支持的其他行为都不在本规范范围内(规范还没有稳定, 使用时需要注意兼容性)
- pan-left, pan-right, pan-up, pan-down: pan-x, pan-y的子集


`touch-action`将如何响应用户操作的决定权交给页面设计者, 这才是解决300ms延迟的最佳方案.


# 当前如何避免延迟

目前没有一个简单方案解决这个问题, 现有的解决方案分为两种类别:

- 指针事件polyfill
- fast click

## 指针事件polyfill

指针事件polyfill有以下代表:

- [jquery PEP][9]
- 微软[HandJS][10]
- [@Rich-Harris][12]的[Points][11]

这些polyfill最难的地方在于在飞IE浏览器中模拟`touch-action`, 由于浏览器会忽略不支持的CSS属性, 唯一能够检测开发者是否声明了
`touch-action: none`的办法就是JavaScript请求并解析所有样式表. HandJS就是这样做的, 但是实现起来在性能上很难做好.

PEP通过判断标签上的`touch-action`属性, 相对来说会简单一些

对于开发者来说如果对指针事件感兴趣, 上面的polyfill就非常合适, 如果只是想寻找解决300ms延迟的方法, 上面的方案就over skill了. 因为他们都是资源密集型的方案. 接下来我们看看专门为了解决300ms而生的解决方案.

## FastClick

[FastClick][13]是专门为解决移动端浏览器300ms延迟而开发的轻量级库. 它检测到`touchend`时, 立即触发`click`时间, 并把浏览器之后
触发的`click`事件阻止掉


# 总结

眼看翻译差不多了才发现部门已经有人翻译过了, 还是一个部门的...[http://thx.github.io/mobile/300ms-click-delay][14]后面就直接
看他翻译的, 然后抄了些过来

# 参考资料

- [http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay][1]
- [https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en][2]

[14]: http://thx.github.io/mobile/300ms-click-delay
[13]: https://github.com/ftlabs/fastclick
[12]: https://github.com/Rich-Harris
[11]: https://github.com/Rich-Harris/Points
[10]: http://handjs.codeplex.com/
[9]: https://github.com/jquery/PEP
[8]: http://caniuse.com/#search=pointer%20event
[7]: https://www.w3.org/TR/pointerevents/
[6]: http://output.jsbin.com/ixibol/6
[5]: https://codereview.chromium.org/18850005/
[4]: https://bugzilla.mozilla.org/show_bug.cgi?id=922896
[3]: https://code.google.com/p/chromium/issues/detail?id=169642
[2]: https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en
[1]: http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay
