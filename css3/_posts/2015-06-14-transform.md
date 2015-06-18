---
title: transform
---

## CSS transform

`transform`可以修改元素的可视化模型，类似于`position: relative;`之后位置偏移，只修改视觉显示，不会影响旁边元素的布局。

先按照常规布局绘制元素，确定布局和显示之后应用transform规则修改视觉效果。

使用`transform`可以对元素视图进行移动，旋转，缩放，skew。

设置`none`意外的取值时，会创建一个[stacking context]，这个上下文会成为内部`position: fixed`元素的包含块。

百分数长度计算依据为元素吃尺寸.

<iframe width="100%" height="600" src="//jsfiddle.net/mse3mtpq/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## transform-origin

设置transform的原点，产生不同效果。

## transform-style

效果和解释: [https://www.webkit.org/blog-files/3d-transforms/transform-style.html][6]

## 2D transform


### rotate(deg)

deg为正时将元素顺时针旋转对应角度，为负时逆时针旋转对应角度。旋转中心为`transform-origin`

如下面的例子：

<iframe width="100%" height="300" src="//jsfiddle.net/otr6adw5/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

旋转矩形后面的灰色矩形模拟元素原始的位置。

### translate(x, y)

根据指定的x，y移动元素


### scale(x, y)

以transform-origin为中心缩放元素，x,y大于1时放大，小于1时缩小。

<iframe width="100%" height="300" src="//jsfiddle.net/spdvmvxn/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### skewX(deg)

以transform-origin为中心，将元素在X轴上倾斜，使元素竖直方向于Y轴产生deg度夹角

### skexY(deg)

以transform-origin为中心，将元素在Y轴上倾斜，使元素水平方向于X轴产生deg度夹角

### matrix(a, b, c, d, e, f)

所有2d transform都可以通过matrix() 实现，window.getComputedStyle()返回的transform对应值也是matirx()表示


## 3D  transform

### rotateX(deg)

以x轴方向为轴旋转元素

<iframe width="100%" height="300" src="//jsfiddle.net/gu3ex5gr/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### rotateY(deg)

以y轴方向为轴旋转元素

<iframe width="100%" height="300" src="//jsfiddle.net/gu3ex5gr/1/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### rotateZ(deg)

以z轴方向为轴旋转元素

<iframe width="100%" height="300" src="//jsfiddle.net/gu3ex5gr/2/embedded/result,css,html" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 参考资料

- [https://dev.opera.com/articles/understanding-the-css-transforms-matrix/][1]
- [http://www.the-art-of-web.com/css/3d-transforms/][2]
- [https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function][3]
- [http://learn.shayhowe.com/advanced-html-css/css-transforms/][4]
- [http://css3files.com/transform/][5]


[6]: https://www.webkit.org/blog-files/3d-transforms/transform-style.html
[5]: http://css3files.com/transform/
[4]: http://learn.shayhowe.com/advanced-html-css/css-transforms/
[3]: https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function
[2]: http://www.the-art-of-web.com/css/3d-transforms/
[1]: https://dev.opera.com/articles/understanding-the-css-transforms-matrix/
