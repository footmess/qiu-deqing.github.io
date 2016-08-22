---
title: spring mvc
---

# jsp中`${name}`输出`${name}`而不是name的值

原因: `web.xml` dtd使用的比较旧
解决方法: 换成高级版本

```
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         id="WebApp_ID" version="3.0">
```





# web app使用maven集成spring mvc

1. `pom.xml`添加依赖

```

```

# 参考资料

- [入门教程: 原理, demo简洁清晰][1]
- [https://crunchify.com/simplest-spring-mvc-hello-world-example-tutorial-spring-model-view-controller-tips/][2]

[2]: https://crunchify.com/simplest-spring-mvc-hello-world-example-tutorial-spring-model-view-controller-tips/
[1]: http://o7planning.org/en/10129/spring-mvc-tutorial-for-beginners
