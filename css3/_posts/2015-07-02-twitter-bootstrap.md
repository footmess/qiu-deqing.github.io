---
title: twitter bootstrap笔记
---

## component

### dropdown

下拉菜单[dropdown][1]三要素

- `position: relative;`的容器用于包含触发按钮和菜单内容(建议使用官方提供的`.dropdown`)
- 设置`data-toggle="dropdown"`的触发按钮
- 设置`class="dropdown-menu"`的菜单

bootstrap会自动寻找到带有data-toggle属性的元素然后绑定点击事件, 为该元素父节点切换`open`类

`dropdown-menu`为绝对定位

```
.dropdown-menu {
  display: none;
}

.open > .dropdown-menu {
  display: block;
}
```


[1]: http://getbootstrap.com/components/#dropdowns
