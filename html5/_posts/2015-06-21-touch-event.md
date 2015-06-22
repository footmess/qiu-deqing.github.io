---
title: touch 事件
---

- [http://www.w3.org/TR/touch-events/][4]
- [Getting touchy - Introduction to touch (and pointer) events / jQuery Europe 2014][6]


## 事件类型


|       事件名称      |         备注       |
|-------------------|--------------------|
| [touchstart][1]   | 触点与触摸屏接触时触发 |
| [touchmove][2]    | 触点在触摸屏上移动时触发 |
| [touchend][3]     | 触点离开触摸屏时在该触点接触触摸屏时的元素上触发 |
| [touchcancel][5]  | fired when a touch point has been disrupted in an implementation-specific manner（如触点太多） |

## Touch对象

Touch对象用于描述触点与触摸屏的一个接触点，具有以下属性：

- `clientX`: 触点相对于viewport的水平像素距离，不包含滚动条偏移
- `clientY`: 触点相对于viewport的竖直像素距离，不包含滚动条偏移
- `identifier`: 触点与触摸屏接触时赋予的唯一标识id，在该触点与触摸屏接触阶段id不变
- `pageX`: 触点相对于viewport的水平像素距离，包含滚动条偏移
- `pageY`: 触点相对于viewport的竖直像素距离，包含滚动条偏移
- `screenX`: 触点相对于屏幕的水平像素距离
- `screenY`: 触点相对于屏幕的竖直像素距离
- `target`: 触点最初与触摸屏接触时的元素

## TouchEvent对象

TouchEvent对象用描述touch事件，包含以下属性：

- `altKey`: 事件发生时alt键是否按下
- `changedTouches: `touchstart`时包含刚与触摸屏接触的触点；`touchemove`时包含上次事件后移动的触点； `touchend`和`touchcancel`时包含离开触摸屏的触点
- `ctrlKey`: 事件发生时ctrl 键是否按下
- `metaKey`: 事件发生时meta键是否按下
- `shiftKey`: 事件发生时shift键是否按下
- `targetTouches`: 触点接触时target为本元素的所有触点

### 与鼠标事件的关系

用户代理可能同时发送touch事件和鼠标事件。如果用户代理同时发送两种事件，`touchstart`事件必须在所有鼠标事件之前发送。在`touchstart`和`touchmove`监听器中如果调用了`preventDefault`方法，用户代理不应该再发送任何的鼠标事件。

如果用户代理将touch事件解析为鼠标事件，那它必须在`touchend`位置按顺序发送`mousemove`,`mousedown`,`mouseup`,`click`事件。


## 如何处理手指在触摸屏上的滑动(swipe)事件

需要监听`touchmove`事件动态判断水平还是竖直滑动。

```
var touchStartClientX,
  touchStartClientY;

document.addEventListener('touchstart', function (event) {
  if (event.targetTouches.length > 1) {
    return; // 忽略多个手指触摸
  }

  touchStartClientX = event.touches[0].clientX;
  touchStartClientY = event.touches[0].clientY;

  event.preventDefault();
}, false);

document.addEventListener('touchmove', function (event) {
  event.preventDefault();
}, false);

document.addEventListener('touchend', function (event) {
  if (event.targetTouches > 0) {
    return; // 忽略多个触点
  }

  var touchEndClientX = event.touches[0].clientX,
    touchEndClientY = event.touches[0].clientY;

  var dx = touchEndClientX - touchStartClientX;
  var absDx = Math.abs(dx);

  var dy = touchEndClientY - touchStartClientY;
  var absDy = Math.abs(dy);

  // 如果移动距离大于10px，认为它是滑动
  if (Math.max(absDx, absDy) > 10) {
    var dir = absDx > absDy ? (dx > 0 ? 'left' : 'right') :
      (dy > 0 ? 'down' : 'up');
  }
}, false);
```

## 300ms延迟

- [https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en][8]


[8]: https://developers.google.com/web/updates/2013/12/300ms-tap-delay-gone-away?hl=en
[7]: http://blog.mobiscroll.com/working-with-touch-events/
[6]: http://www.slideshare.net/redux/getting-touchy-introduction-to-touch-and-pointer-events-jquery-europe-2014-vienna-28022014
[5]: https://developer.mozilla.org/en-US/docs/Web/Events/touchcancel
[4]: http://www.w3.org/TR/touch-events/
[3]: https://developer.mozilla.org/en-US/docs/Web/Events/touchend
[2]: https://developer.mozilla.org/en-US/docs/Web/Events/touchmove
[1]: https://developer.mozilla.org/en-US/docs/Web/Events/touchstart
