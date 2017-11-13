---
title: spring boot
---

基于maven的spring boot web项目

# 常用命令

- `mvn spring-boot:run`: 启动基于spring boot web的项目
- `mvn build`编译,然后进入到`target`目录执行`java -jar xxx.jar`

# 属性配置

`application.properties`是配置文件也可以使用`application.yml`方便配置


# 在配置文件中设置的值在代码中访问方式

1. 直接访问

  ```
  @Value("${name}")
  private String name;
  ```

2. 复杂对象访问

  ```
  girl:
    cupSize: b
    age: 18
  ```

  添加对应Java类如`GirldProperties`

  ```

  @Component
  @ConfigurationProperties(prefix = "girl")
  public class GirlProperties {
      private String cupSize;
      private Integer age;
  }

  // 使用
  @Autowired
  private GirlProperties girlProperties;  // spring自动将配置文件中girl下面的属性读取设置到对象
  ```

# 配置不同开发环境

1. 根据需求编写多个配置文件,如`application-dev.yml`,`application-prod.yml`,在`application.yml`中设置生效的文件:
  ```
  spring:
    profiles:
      active: dev
  ```
  此时启动应用使用的配置文件是`application-dev.yml`

2. 通过命令行参数在应用启动时指定环境:`java -jar target/crm-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod`此时使用的配置文件是`application-prod.yml`


# 参考资料

- [Spring Boot 那些事][1]


[1]: https://www.bysocket.com/?page_id=1639
