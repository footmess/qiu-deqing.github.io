---
title: 触摸元素状态
---

在PC上经常使用`:hover`为用户鼠标滑过提供反馈, 无线浏览器没有hover
状态, 触目元素时需要提供反馈可以使用`:active`状态.

所有无线浏览器都支持`<a>`元素的active状态, 其他元素有兼容性问题.

```
a:active {
    background-color: #333;
}
```
