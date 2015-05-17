---

title: 原生拖拽,拖放事件(drag and drop)

---

# 原生拖拽,拖放事件(drag and drop)

拖拽,拖放事件可以通过拖拽实现数据传递,达到良好的交互效果,如:从操作系统拖拽文件实现文件选择,拖拽实现元素布局的修改.

## drag and drop事件流程

一个完整的drag  and drop流程通常包含以下几个步骤:

1. **设置可拖拽目标**.设置属性`draggable="true"`实现元素的可拖拽.
2. **监听`dragstart`设置拖拽数据**
3. **为拖拽操作设置反馈图标(可选)**
4. **设置允许的拖放效果**,如`copy`,`move`,`link`
5. **设置拖放目标**,默认情况下浏览器阻止所有的拖放操作,所以需要**监听`dragenter`或者`dragover`取消浏览器默认行为使元素可拖放**.
6. **监听`drop`事件执行所需操作**

## 拖拽事件

以下是拖拽产生的一系列事件,拖拽事件产生时不会产生对应的鼠标事件.

- `dragstart`:拖拽开始时在被拖拽元素上触发此事件,监听器需要设置拖拽所需数据,从操作系统拖拽文件到浏览器时不触发此事件.
- `dragenter`:拖拽鼠标进入元素时在该元素上触发,通过取消浏览器默认行为可以使远处成为可拖放目标.如果元素为拖放目标,通常需要设置一些视觉效果提示用户元素可拖放.
- `dragover`:拖拽时鼠标在目标元素上移动时触发.多数情况下,可以执行的操作与`dragenter`相同.
- `dragleave`:拖拽时鼠标移出目标元素时在目标元素上触发.此时监听器可以取消掉前面设置的视觉效果.
- `drag`:拖拽期间在被拖拽元素上连续触发
- `drop`:鼠标在拖放目标上释放时,在拖放目标上触发.此时监听器需要收集数据并且执行所需操作.
- `dragend`:鼠标在拖放目标上释放时,在拖拽元素上触发.将元素从浏览器拖放到操作系统时不会触发此事件.

## DataTransfer对象

拖拽事件周期中会初始化一个`DataTransfer`对象,用于保存拖拽数据和交互信息.以下是它的属性和方法.

- `dropEffect`: 拖拽交互类型,通常决定浏览器如何显示鼠标光标并控制拖放操作.常见的取值有`copy`,`move`,'link'和`none`
- `effectAllowed`: 指定允许的交互类型,可以取值:`copy`,`move`,`link`,`copyLink`,`copyMove`,`limkMove`, `all`, `none`默认为`uninitialized`(允许所有操作)
- `files`: 包含`File`对象的`FileList`对象.从操作系统向浏览器拖放文件时有用.
- `types`: 保存`DataTransfer`对象中设置的所有数据类型.
- `setData(format, data)`: 以键值对设置数据,format通常为数据格式,如`text`,`text/html`
- `getData(format)`: 获取设置的对应格式数据,format与setData()中一致
- `clearData(format)`: 清除指定格式的数据
- `setDragImage(imgElement, x, y)`: 设置自定义图标

`dataTransfer`对象在传递给监听器的事件对象中可以访问,如下:

```
draggableElement.addEventListener('dragstart', function (event) {
  event.dataTransfer.setData('text', 'Hello World');
}, false);
```

## 参考资料

- [https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Drag_and_drop]()
- [https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Drag_operations]()
- [http://blog.teamtreehouse.com/implementing-native-drag-and-drop]()
- [http://www.html5rocks.com/en/tutorials/dnd/basics/]()
