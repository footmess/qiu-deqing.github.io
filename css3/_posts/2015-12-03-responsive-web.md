---
title: 响应式web
---

## 目标

在任意设备上灵活展示页面, 最小化水平滚动和缩放

## 图片

### iconfont

- 尺寸灵活, 不失真
- 文件小
- 只适合纯色

### 普通图片

- 设置最大宽度,高度自动

```
max-width: 100%;
height: auto;
```

### 背景图片

- 支持双倍图

  ```
  background-image: url("//gtms03.alicdn.com/tps/i3/TB1LoVGKpXXXXXOXFXXXWVYJXXX-79-84.png");
  background-image: -webkit-image-set(
                      url("//gtms03.alicdn.com/tps/i3/TB1LoVGKpXXXXXOXFXXXWVYJXXX-79-84.png") 1x,
                      url("//gtms04.alicdn.com/tps/i4/TB1U8XoKpXXXXcUXVXX0gTDPpXX-158-168.png") 2x
                    );
  ```

- 宽度变化高度不能自适应: 1. rem 2. background-size

  ```
  width: 3rem;
  height: 2rem;
  backgrond-size: 100% 100%;
  ```


[1]: https://davidwalsh.name/responsive-images
