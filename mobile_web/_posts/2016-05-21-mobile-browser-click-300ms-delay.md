---
title: 无线端浏览器click事件300ms延迟
---


# 起因

2007年苹果为了解决iPhone Safari访问PC页面的缩放问题, 提供了一个**双击缩放**功能.当用户第一次触摸屏幕的时候浏览器等待300ms才能判断用户是要点击(click)还是缩放(zoom),这就造成用户触摸屏幕到click事件触发存在300ms延迟.

触摸屏幕事件流程如下:

1. touchstart
2. touchend
3. Wait 300ms in case of another tap
4. click

受到这个延迟影响的场景有:

- JavaScript监听的click事件
- 基于click的交互的元素, 例如链接, 表单元素

随着iPhone的成功, 后续的无线浏览器复制了它的大部分操作习惯, 其中就包括双击缩放, 这也成为主流浏览器的一个功能.

这300ms延迟在平时浏览网页不会带来严重问题, 但是对于高性能web app就是一个严重的问题, 响应式设计的流行也让双击缩放逐渐失去用武之地.

[点击本页面测试click延迟][6]

# 解决方案

各大浏览器厂商尝试去掉300ms延迟, 但是这对于Safari来说几乎不可能, 因为在Safari上双击还有其他浏览器没有的滚动功能. 以下是一些浏览器提供的解决方案:

- 取消禁止缩放页面的延迟
    双击是为了缩放页面, 在不能缩放的页面禁止延迟也就理所当然, 安卓版的Chrome, Opera, BlackBerry和Firefox在设置以下meta标签的页面中取消了延迟

    ```
    <meta name="viewport" content="user-scalable=no">
    ```

- 取消为无线设计页面的延迟
    考虑到禁止缩放会带来可用性和可访问性的问题, Chrome 32开始[取消viewport设置为width小于等于device-width页面两次敲击缩放功能][5], Opera, UC9也都实现了这一功能, Firefox还没有实现.

    ```
    <meta name="viewport" content="width=device-width">
    ```

- 指针事件(Pointer Event)
    指针事件使用一套事件模型统一鼠标, 触屏, 笔触等所有输入事件, 其中引入的CSS属性`touch-action`将如何响应用户操作的决定权交给页面设计者, 这才是解决300ms延迟的最佳方案. 取值和含义如下:

    - auto(默认): 有用户代理决定如何响应用户的行为(如滚动, 缩放等)
    - none: 对用户行为不触发任何反应
    - pan-x: 用户代理认为用户的触屏操作目的是水平滚动元素最近可水平滚动的祖先元素
    - pan-y: 用户代理认为用户的触屏操作目的是竖直滚动元素最近可竖直滚动的祖先元素
    - manipulation: 用户代理认为用户触屏操作只是为了滚动和连续缩放. `auto`所支持的其他行为都不在本规范范围内(规范还没有稳定, 使用时需要注意兼容性)
    - pan-left, pan-right, pan-up, pan-down: pan-x, pan-y的子集


    ```
    html {
        -ms-touch-action: manipulation;
            touch-action: manipulation;
    }
    ```


- 监听touchend事件
    `touchend`不会有延迟, 感觉替代`click`完全可以, 但是存在一些问题:

    - 响应式页面在PC上需要依赖`click`事件, 同时监听两种事件会导致任务执行两次.

    在页面功能简单的时候可以采用这个方案, 如果复杂了就不好管理控制.

- 指针事件polyfill
    指针事件polyfill有以下代表:

    - [jquery PEP][9]
    - 微软[HandJS][10]
    - [@Rich-Harris][12]的[Points][11]

    这些polyfill最难的地方在于在飞IE浏览器中模拟`touch-action`, 由于浏览器会忽略不支持的CSS属性, 唯一能够检测开发者是否声明了`touch-action: none`的办法就是JavaScript请求并解析所有样式表. HandJS就是这样做的, 但是实现起来在性能上很难做好.

    PEP通过判断标签上的`touch-action`属性, 相对来说会简单一些

    对于开发者来说如果对指针事件感兴趣, 上面的polyfill就非常合适, 如果只是想寻找解决300ms延迟的方法, 上面的方案就over skill了. 因为他们都是资源密集型的方案. 接下来我们看看专门为了解决300ms而生的解决方案.

- FastClick
    [FastClick][13]是专门为解决移动端浏览器300ms延迟而开发的轻量级库. 它检测到`touchend`时, 立即触发`click`时间, 并把浏览器之后触发的`click`事件阻止掉, 在不存在300ms的浏览器上它什么也不做. 目前来说FastClick是最简单实用的方案.


# 总结

结合前面的讨论个人采用以下方案

- 设置viewport
    ```
    <meta name="viewport" content="width=device-width, initial-scale=1">
    ```
- 设置`touch-action`
    ```
    html {
        -ms-touch-action: manipulation;
            touch-action: manipulation;
    }
    ```
- 引入FastClick
    ```
    if ('addEventListener' in document) {
        document.addEventListener('DOMContentLoaded', function() {
            FastClick.attach(document.body);
        }, false);
    }
    ```

# 参考资料

- [http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay][1]
- [https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en][2]
- [http://www.quirksmode.org/blog/archives/2014/04/suppressing_the.html][15]
- [https://webkit.org/blog/5610/more-responsive-tapping-on-ios/][16]

[16]: https://webkit.org/blog/5610/more-responsive-tapping-on-ios/
[15]: http://www.quirksmode.org/blog/archives/2014/04/suppressing_the.html
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
