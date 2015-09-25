---
title: 实现整屏滚动组件
---

[整屏滚动][4]是web常见的交互模型, 写成组件可以减少重复工作. github上有现成的[fullPage.js][1]实现了这个功能, 可以直接使用, 本文我们一步步实现这样一个组件. 组件依赖于jQuery.

## 基本思路

- 设置页面高度为窗口高度
- 要显示

## UMD

我们的组件计划支持全局对象访问, AMD模块加载方式, [UMD][2]总结了支持各种模块规范的组件编写方式, 我们根据需要选择了[amdWeb.js][3]. `full-page-scroll.js`框架如下:

```
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        define(['jquery'], factory);
    } else {
        root.FullPageScroll = factory(root.jQuery);
    }
}(this, function ($) {

    function FullPageScroll() {}

    return FullPageScroll;
}));
```

测试文件index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="initial-scale=1.0, width=device-width, user-scalable=no">
  <title>整屏滚动</title>
</head>
<body>

<style>
body {
  margin: 0;
}
.page {
  height: 800px;
}
.page1 {
  background-color: #aec7e8;
}
.page2 {
  background-color: #98df8a;
}
.page3 {
  background-color: #c5b0d5;
}
.page4 {
  background-color: #c7c7c7;
}
</style>

<div class="page page1">1</div>
<div class="page page2">2</div>
<div class="page page3">3</div>
<div class="page page4">4</div>
<div class="page page5">5</div>

<script src="//cdn.bootcss.com/require.js/2.1.20/require.min.js"></script>

<script>
require.config({
  paths: {
    jquery: '//g.tbcdn.cn/mm/activity-assets/20150909.162311.437/jquery-1.11.3.min'
  }
});

require(['full-page-scroll'], function (FullPageScroll) {
  console.log(FullPageScroll);
});
</script>

</body>
</html>
```

将`index.html`和`full-page-scroll.js`放在同一个目录下启动服务器访问index.html可以看到控制台输出

```
FullPageScroll() {}
```

表示框架搭建完成.

## 设置默认参数并支持用户输入覆盖

为`FullPageScroll`添加`defaults`属性并在构造函数中将用户参数与默认参数合并.

```
function FullPageScroll(options) {
    $.extends(true, this, FullPageScroll.defaults, options);
}

FullPageScroll.defaults = {
    current: 0
};
```

## 设置需要管理的页面,并设置页面高度等于窗口高度

组件管理的页面通过构造函数参数中`pages`属性输入. 添加`cacheElements`方法获取需要的元素并缓存:

```
/**
 *  保存组件运行需要的元素
 **/
SinglePageScroll.prototype.cacheElements = function () {
  var me = this;
  me.doc = $(document);
  me.win = $(window);
  me.body = $('body');

  if (!(pme.pages && me.pages.length)) {
    throw new Error('页面列表不能为空');
  }
};
```

然后添加`adjustPageSize`

[4]: #滚屏demo url
[3]: https://github.com/umdjs/umd/blob/master/amdWeb.js
[2]: https://github.com/umdjs/umd
[1]: https://github.com/alvarotrigo/fullPage.js
