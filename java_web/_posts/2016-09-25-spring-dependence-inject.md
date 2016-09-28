---
title: spring依赖注入
---

# 隐式bean扫面和自动封装

核心工作包含两个方面:

- 组件扫描
- 自动封装

1. Bean设置`@Component`注解

        package com.qiudeqing;

        import org.springframework.stereotype.Component;


        @Component
        // @Component("beanName")
        public class SgtPeppers implements CompactDisc {

            public void play() {
                System.out.println("hello");
            }

        }


2. 新建配置类, 设置@Configuration标记为配置类, 设置@ComponentScan包路径

        package com.qiudeqing;

        import org.springframework.context.annotation.ComponentScan;
        import org.springframework.context.annotation.Configuration;

        @Configuration
        @ComponentScan
        // @ComponentScan(basePackages = {"com.qiudeqing"})
        // @ComponentScan(basePackageClasses = {CDPlayer.class, DVDPlayer.class}) 推荐
        public class CDConfig {
        }

3. 为Bean自动注入所依赖的类

        package com.qiudeqing;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Component;

        @Component
        public class CDPlayer implements MediaPlayer {
            private CompactDisc cd;

            @Autowired(required = false)
            public CDPlayer(CompactDisc cd) {
                this.cd = cd;
            }

            public void play() {
                cd.play();
            }
        }



# java显式配置

1. Bean配置和之前一样, 添加@Component

2. 和自动配置一样添加配置类, 设置@Configuration注解, 不设置@ComponentScan, 添加@Bean方法


        package com.qiudeqing;

        import org.springframework.context.annotation.ComponentScan;
        import org.springframework.context.annotation.Configuration;

        @Configuration
        public class CDConfig {

            @Bean(name = "beanName")
            public CompactDisc compactDisc() {
                return new SgtPeppers();
            }
        }

3. 为Bean注入依赖Bean


        @Bean CDPlayer cdPlayer(CompactDisc compactDisc) {
            return new CDPlayer(compactDisc)
        }


# xml显式配置

1. 编写普通java类, 在classpath下添加配置文件如`applicationContext.xml`

        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
                    http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context">
            // bean 配置
        </beans>

2. 添加普通Bean

        <bean class="com.qiudeqing.SgtPeppers" />

3. 通过构造函数传递Bean, value

        <bean class="com.qiudeqing.Book" id="helloBook">
            <constructor-arg name="title" value="hello"/>
            <constructor-arg ref="compactDisc"/>
        </bean>

4. 通过属性注入Bean依赖

        <bean class="com.qiudeqing.Book" id="helloBook">
            <property name="compactDisc" ref="compactDisc" />
            <property name="title" value="hello" />
        </bean>


# java配置文件中引用其他java配置

@Configuration
@Import({CDConfig.class, HelloConfig.class})
public class CDPlayerConfig {
    @Bean
    public CDPlayer cdPlayer(CompactDisc compactDisc) {
        return new CDPlayer(compactDisc);
    }
}

# java配置文件中引入xml配置文件

@Configuration
@import(CDPlayerConfig.class)
@inportResource("classpath:cd-config.xml")
public class SoundSystemConfig {

}

# xml配置文件中引入xml配置文件

    <import resource="cd-config.xml" />

# xml配置文件中引入java配置

只需要添加配置文件所在java文件为bean即可

    <bean class="com.qiudeqing.CDConfig" />

# 设置根配置方便管理

不管是使用java类还是xml配置, 设置一个根配置文件, 在里面引入各个模块配置, 包扫描路径.
