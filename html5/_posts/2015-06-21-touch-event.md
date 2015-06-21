---
title: touch 事件
---

[http://www.w3.org/TR/touch-events/][4]


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


[5]: https://developer.mozilla.org/en-US/docs/Web/Events/touchcancel
[4]: http://www.w3.org/TR/touch-events/
[3]: https://developer.mozilla.org/en-US/docs/Web/Events/touchend
[2]: https://developer.mozilla.org/en-US/docs/Web/Events/touchmove
[1]: https://developer.mozilla.org/en-US/docs/Web/Events/touchstart
