---

title: iframe标签使用

---


## iframe标签使用

[HTML inline Frame Element(<iframe>)][1]表示一个嵌套上下文,用于在页面中嵌套其他页面.可以在文档流中任何地方放置iframe,如:[iframe例子][8]

### iframe基本例子

以下代码设置iframe的最基础属性:

    <iframe src="http://qiudeqing.com/html5/2015/05/25/iframe-tutorial.html"></iframe>

其中`src`属性指定iframe要展示文档的url. 尽管可以使用width, height, scrolling, frameborder属性来控制iframe的显示, 我们推荐使用css来控制样式.

    iframe {
      border: 1px solid #ccc;
      width: 80%;
      height: 120px;
    }

当加载文档高于iframe设置的高度时,会自动生成滚动条,可以通过使用javascript动态设置iframe高度来避免滚动条的出现.

可以使用CSS和JavaScript来修改iframe的样式和数学,例如:位置,尺寸,src.还可以使用JavaScript在文档之间传递数据.

**校验**: 包含iframe的文档可以通过HTML5校验,不能通过严格XHTML和严格HTML4.可以通过transitional XHTML和HTML4校验.一些属性在HTML4中是合法的,但是在HTML5中已经不合法.

**iframe文档样式**: iframe加载的文档不会从包含它的文档中继承样式.iframe内部的样式由它内部的CSS控制.

**iframe与链接**: New documents can be loaded into iframes using the link target attribute or JavaScript. Links inside iframes load new documents into the iframe by default, but you can set the target attribute to _parent to replace the containing document. See an example.

## 属性

iframe包含所有[全局属性][2],以下是它自有的(仅列出未废弃的和兼容性好的属性).

- `height`: 设置frame的高度数,HTML5中单位为CSS像素,HTML4.01中可以是像素或者百分数. css中也可以设置元素高度.
- `name`: A name for the embedded browsing context (or frame). This can be used as the value of the target attribute of an `<a>` or `<form>` element, or the formtarget attribute of an `<input>` or `<button>` element.
- `src`: 需要嵌入的页面的URL.
- `width`: 指定元素宽度,规则和高度一样

## 使用JavaScript或者链接为iframe加载新文档

可以设置链接的`target`属性来实现点击链接后在对应iframe中加载链接所指向的文档.[在线demo][9].

## 脚本操作

可以使用普通`document.querySelector()`获取页面中指定的iframe元素,也可以通过[window.frames][3]获取当前浏览器窗口下内嵌的所有frame的类数组表示.通过`window.frames[i]`可以获取iframe元素,`iframe.contentWindow`获取iframe对应的window对象.

在frame内部,通过[window.parent][4]可以访问包含它的父窗口.

脚本访问frame内容时会受到[same-origin policy][5]的限制.需要跨域通信时可以使用[window.postMessage][6].

## 常见操作

- **去掉默认边框**: 默认情况下iframe有1px的边框, HTML4中通过`frameborder="0"`可去掉边框,但是这个属性已经废弃,推荐在css中使用`border: 0`去掉边框.


## 参考资料

- [http://www.dyn-web.com/tutorials/iframes/][7]


[1]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe
[2]: https://developer.mozilla.org/en-US/docs/HTML/Global_attributes
[3]: https://developer.mozilla.org/en-US/docs/Web/API/Window/frames
[4]: https://developer.mozilla.org/en-US/docs/Web/API/Window/parent
[5]: https://developer.mozilla.org/en-US/docs/Same_origin_policy_for_JavaScript
[6]: https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage
[7]: http://www.dyn-web.com/tutorials/iframes/
[8]: http://qiudeqing.com/demo/html5/iframe-tutorial.html#d1


