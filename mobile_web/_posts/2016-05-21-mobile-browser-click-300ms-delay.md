---
title: 无线端浏览器click事件300ms延迟
---

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

Chrome团队宣布从Chrome 32开始[取消无线页面两次敲击缩放功能来消除300ms延迟]


# 参考资料

- [http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay][1]
- [https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en][2]


[5]: https://codereview.chromium.org/18850005/
[4]: https://bugzilla.mozilla.org/show_bug.cgi?id=922896
[3]: https://code.google.com/p/chromium/issues/detail?id=169642
[2]: https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en
[1]: http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay
