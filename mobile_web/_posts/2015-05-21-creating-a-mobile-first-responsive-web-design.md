---

title: "[译]创建一个Mobile First的响应式页面"

---

## 介绍

[原文链接][1]

本文将演示如何创建一个mobile-first的响应式web页面,本文和[demo][2]将覆盖以下内容:

**可以在[Web Fundamentals][3]找到更多最新的响应式指导资料**

- 为什么要创建mobile-first,响应式,自适应用户体验页面
- 如何创建性能优化和灵活性优先的自适应页面HTML结构
- 如何编写共享样式优先的样式,然后再使用media query为大屏幕设置样式,如何使用相对单位
- 如何编写非侵入式的javascript来条件性加载内容,利用触摸事件和地理API
- 如何增强自适应用户体验

## 自适应的需要

随着设备尺寸的多样化,提供良好的用户体验变得更加重要.幸亏[响应式web设计][4]web作者提供了为不同尺寸提供布局的方法.我们将使用fluid grid, flexible image和media query完成需要的功能.

除了屏幕尺寸之外,移动设备接入网络的带宽也经常变化,从WIFI到3G网络.移动设备的触屏也提供了新的交互模式.

我们将为移动环境创建一个页面,这个页面不仅仅是针对小屏幕,同时会考虑移动web开发所面临的各种挑战.移动环境的限制要求我们尽快将重要内容提供给用户.创建**快速加载**,**优化的移动设备有限用户体验**越来越重要.

## 本教程实现什么: 产品详情页

我们将为一个T恤公司的产品提供详情页.

## 设置viewport

使用[viewport meta tag][5]设置屏幕宽度大小等于设备宽度.

    <meta name="viewport" content="width=device-width, initial-scale=1">

需要注意的是我们没有禁止用户对页面的缩放功能,这样能方便用户根据需求调整页面.有些情况下需要禁止用户缩放,比如[fixed positioned elements][6]

## 内容段

为了保证用户快速加载内容,提高用户体验.我们把不重要的内容放到`reviews.html`和`related.html`文件中.默认情况下不需要加载,我们将[conditionally load the content][7].

## HTML特殊字符

我们使用HTML特殊字符表示图标,避免了图片的使用,其中`&#9733;`用于创建&#9733;, `&#9734`用于创建&#9733;.

## tel: URI Scheme

通过为链接使用tel URI实现用户点击链接即可拨打电话的功能:

    <a href="tel:+18005550299">1-800-555-0199</a>

## 样式

在设计CSS的适合我们尽量保持轻量级和流式布局.

## 将为大屏幕准备的样式分开

我们将创建两个单独的CSS文件.`style.css`和`enhanced.css`其中一个屏幕小于40.5em时使用默认样式,宽于40.5em时使用media query添加增强样式.

    <link rel="stylesheet" href="style.css" media="screen, handheld">
    <link rel="stylesheet" href="enhanced.css" media="screen and (min-width: 40.5em)">
    <!--[if (lt IE 9)&(!IEMobile)]>
    <link rel="stylesheet" type="text/css" href="enhanced.css" />
    <![endif]-->

我们使用em作为断点是为了让内容来决定断点.

## 移动设备优先的样式

从基础共享样式开始,逐步添加高级布局会让代码变得简洁,可维护,如下所示:

    /* large screen styles first: 避免这样使用 */
    .product-img {
      width: 50%;
      float: left;
    }
    @media screen and (max-width: 40.5em) {
      .product-img {
        width: auto;
        float: none;
      }
    }

我们需要劲量避免代码的复杂性,下面是移动设备优先的样式:

@media screen and (min-width: 40.5em) {
  .product-img {
    width: 50%;
    float: left;
  }
}

在这里我们不是先声明大尺寸屏幕下的样式,然后在小屏幕下覆盖它,我们只是针对宽屏幕进行了覆盖.页面布局默认情况下就是流式的所以我们尽可能利用这一特点.需要注意的是一些移动端浏览器不支持media query,使用mobile first可以更好地照顾这些设备.


## 使用media query

我们使用media为更宽的屏幕提供优化样式

/* default styles */
.related-products li {
  flaot: left;
  width: 50%;
}

/* display 3 per row for medium displays */
@media screen and (min-width: 28.75em) {
  .related-products li {
    width: 33.333333%;
  }
}

/* display 6 to a row for large displays */
@media screen and (min-width: 40.5em) {
  .related-products li {
  width: 16.66666667%;
  }
}

这样可以随着屏幕的宽度增加在没一行显示更多元素

## 使用相对单位

我们使用百分数和em来尽量确保灵活性,相对单位在不同屏幕大小,像素密度,缩放级别上都具有更好的显示效果.

虽然media query是响应式设计的利器,我们需要尽可能使用[fluid grids][8]完成工作.

    result = target / context

## 使用CSS减少请求

太多的HTTP请求会成为[huge killer for performance][9],特别是在移动设备上.我们将劲量使用CSS技术减少HTTP请求书来提高性能.使用[CSS gradients][10]代替背景图片是一个很好的例子.我们需要使用恰当的厂商前缀来尽量满足兼容性.



[1]: http://www.html5rocks.com/en/mobile/responsivedesign/
[2]: http://qiudeqing.com/demo/mobile_web/create-a-responsive-web.html
[3]: https://developers.google.com/web/fundamentals/layouts/
[4]: http://www.alistapart.com/articles/responsive-web-design/
[5]: https://developer.apple.com/library/ios/#DOCUMENTATION/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html
[6]: http://bradfrost.com/blog/mobile/fixed-position/
[7]: http://24ways.org/2011/conditional-loading-for-responsive-designs
[8]: http://www.alistapart.com/articles/fluidgrids/
[9]: http://speakerdeck.com/u/tkadlec/p/optimizing-for-mobile-performance
[10]: http://css-tricks.com/css3-gradients/
[]:
[]:
[]:
[]:
[]:
[]:
[]:
