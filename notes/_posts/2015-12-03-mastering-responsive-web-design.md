---
title: Mastering Responsive Web Design笔记
---

![][1]

# 第一章 利用SASS特性更好地实现响应式web

- 安装ruby版本的sass
- 目录下创建`scss`, `css`两个目录, 执行命令`sass --watch scss:css`会监控scss文件修改并编译到css目录


## Sass基本概念

### Sass vs SCSS

Sass有Sass和SCSS两种语法.

Sass语法也叫做缩进语法, 是最初的Sass语法, 不使用任何的括号和分号, 缩进规则非常严格并且是强制的.

Sass 第三版引入了SCSS语法, 在CSS的基础上进行了增强

### Sass变量

Sass变量必须以`$`符号开头, 修改变量值将替换所有使用到这个变量的值. 变量后面的`;`用于分隔多个变量声明.

变量声明应该放到SCSS文件的开头

```
$brandBlue: #aaa;
$brandColor: #eee;

body {
  color: $brandColor;
}
```

### Sass mixins

Mixin是一系列可以服用的CSS声明, 类似于变量.

- 以`@mixin`指令开头
- 使用`@include`指令调用
- 通过参数可以是mixin更加强大

```
$brandBlue: #333;
$supportGray: #ccc;

@mixin genericContainer {
  padding: 10px;
  border: $brandBlue 1px solid;
  background: $supportGray;
  box-shadow: 1px 1px 1px rgba(black, 0.5);
}

.selector-a {
  @include genericContainer;
}
```

### Sass参数

通过参数使mixin更加强大. 支持默认参数

```
$brandBlue: #ddd;
$supportGray: #ccc;
@mixin genericContainer($padding: 8px, $bdColor: orange) {
  padding: $padding;
  border: $brandBlue 1px solid;
  background: $bdColor;
  box-shadow: 1px 1px 1px rgba(black, 0.3);
}

.selector-a {
  @include genericContainer(10px);
}
.selector-b {
  @include genericContainer($bdColor: $brandBlue);
}
```

### 嵌套

像HTML一样嵌套选择器, 生成对应嵌套选择器.

```
$brandBlue: #ddd;

nav {
  ul {
    display: flex;
    color: $brandBlue;
  }
}
```

### partial

partial是一小段SCSS代码, 方便我们实现模块化, 如`_variables.scss`. partial以下划线开头, `.scss`为后缀. 下划线告诉编译器不要把文件编译为单独的css.

使用`@import`指令引入partial, 不需要指定下划线和文件后缀名.

_variables.scss:

```
$brandBlue: #416e8e;
$brandRed: #c03;
$brandYellow: #c90;
```

在styles.scss中引入_variables.scss:

```
@import "variables";
```

### extend/inherit

extend允许你直接使用另一个选择器的属性. 通过`@extend`指令完成.

```
$brandBlue: #ddd;
.generic-container {
  padding: 10px;
  border: $brandBlue 1px solid;
}

.box-customer-service {
  @extend: .generic-container;
  padding: 25px;
}
```

输出结果:

```
.generic-container, .box-customer-service {
  padding: 10px;
  border: #ddd 1px solid;
}

.box-customer-service {
  padding: 25px;
}
```

### Sass注释

`//`注释编译之后不输出到css

`/**/`注释编译之后输出到css

### 浏览器前缀

- `-moz-` Mozilla
- `-o-` Opera
- `-ms-` Microsoft
- `-webkit-` Webkit(Apple)
- `-webkit-` Chrome

Chrome最初使用Webkit内核, 后来改用Blink, 但是还是使用`-webkit-`

Opera最初使用Presto,然后Webkit,然后Blink

可以使用[Compass][2]处理前缀.

也可以用[-prefix-free][3]



[3]: http://leaverou.github.io/prefixfree/
[2]: http://compass-style.org/
[1]: http://gtms04.alicdn.com/tps/i4/TB1C9hjKVXXXXbsXpXXNuAVQVXX-405-500.jpg

