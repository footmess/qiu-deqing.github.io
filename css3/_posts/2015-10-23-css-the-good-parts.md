---
title: CSS最佳实践
---

[原文][1]

## Namespaces

组件**必须**设置唯一的命名空间做为前缀

- 命名空间必须短并且有意义
- 用`-`分隔命名空间和元素
- 视图应该由多个单独组件构成

比如一个**drop down list**组件使用`ddl`作为命名空间:

**Good**

```
.ddl-container {}

.ddl-item-list {}

.ddl-item {}
```

**Bad**

```
.item-list {}

.dropdown-item-list {}

.xyz-item-list {}
```

通用的类提供可复用样式应该放在单独命名空间如`uv`

**Good**

```
.uv-clearfix {}
```

**Bad**

```
.clearfix {}
```

## Classes

类名应该遵循以下规则

- 必须小写
- 用`-`分隔词
- **尽量短**, 缩写时注意
- 命名一致性
- 语义化描述元素
- 低于4个单词

**Good**

```
.ddl-item {}

.ddl-selected {}

.ddl-item-selected {}
```

**Bad**

```
.ddlItem {}


.ddl-item-container-text {}

.ddl-foo-bar-baz {}
```

## Attributes

属性选择器经常带来好处, 基础规则:

- 如果只检查属性是否设置可以满足要求, 就只检查属性是否设置
- 不要使用标签过分修饰

**Good**

```
[href] {}
```

**Bad**

```
a[href] {}

[href^='http'] {}
```

## `id` 选择器

**禁止**使用id选择器

- id选择器不可复用
- 优先级噩梦

**Good**

```
.ur-name {}
```

**Bad**

```
#ur-name {}
```

## Tag Names

标签选择器使用应遵循以下规则

- 需要在整个应用级别上覆盖元素默认样式时可以使用
- **尽量使用类代替标签选择器**
- 如果在同一个命名空间下有很多元素需要使用相同的样式, 可以使用标签选择器
- **不要过分修饰**: `a.foo`

**Good**

```
button {
  padding: 5px;
  margin-right: 3px;
}
```

**Bad**

```

```

[1]: https://github.com/bevacqua/css
