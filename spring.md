# Spring

[TOC]

## Spring Boot

* 快速开发工具，可以开发单个微服务
* 约定大于配置

## Spring Cloud

* Spring cloud基于Springboot实现

SpringBoot前提是了解SpringMVC。

### IOC思想

在应用代码不再new对象，而是通过容器new对象，让代码模块之间解耦合。新增功能时尽量不用修改旧有代码，直接更改配置文件。

### Constructor Argument Resolution

```
\obj\obj_java\SpringStudy\spring_ioc_03
```

1. constructor-arg index
2. constructor-arg type
3. constructor-arg name

### Spring配置

1. alias别名
2. bean配置
3. import导入

#### alias

```
    <alias name="user" alias="user4"/>
```

#### bean配置

```
    <bean id="user5" class="org.zero0xff.pojo.User" name="user6,user7">
        <constructor-arg name="name" value="user5,user6,user7"/>
    </bean>
```

#### import

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="beans.xml"/>
    <import resource="beans1.xml"/>
    <import resource="beans2.xml"/>
</beans>
```

## 依赖注入DI

1. 构造其注入
2. set注入（重点）
3. 扩展方式注入

### 构造注入

Constructor Argument Resolution 

### set注入

1. 依赖：bean对象创建所需的对象
2. 注入：bean对象创建所需要的对象由容器注入

### Bean作用域

1. singleton 单例模式
2. prototype

## Bean自动装配方式

1. JAVA代码装配
2. xml装配
3. 注解装配

### xml自动装配

1. ByName自动装配
2. ByType自动装配

### 注解自动装配

1. 导入约束

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:annotation-config/>
    <!--    制定要扫描的包，包下的注解生效-->
    <context:component-scan base-package="org.zero0xff.pojo"/>

</beans>
```



1. 引用注解

#### @Atuowired

直接在属性上使用，也可以在set方式上使用。使用后可以不写set方法。默认采用名字自动构建。也可以使用@Qualifier(value="[id]")制定bean进行装配。

#### @Resource

```
@Resource(name="cat2")
private Cat cat；
```

#### @Component

* @Service
* @Controller
* @Repository
* @Component

以上4个注解功能都一样，向容器注册类。



### 总结

* xml更加方便万能，适用于各种场景
* 注解不是自己的类使用不了，维护相对负杂

最佳实践

* xml用来管理bean。
* 注解负责完成属性注入。

## 使用JAVA配置Spring

JavaConfig是Spring的子项目，在Spring4之后，JavaConfig已经变成Spring核心功能。

```java
package org.zero0xff.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;
import org.zero0xff.pojo.User;

@Configuration
@ComponentScan("org.zero0xff.pojo")
public class MyConfig {
    @Bean
    public User getUser() {
        return new User();
    }
}
```



```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.zero0xff.config.MyConfig;
import org.zero0xff.pojo.User;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = context.getBean("getUser", User.class);
        System.out.println(user.toString());
    }
}
```

## 动态代理

* JKD动态代理
* 基于cglib动态代理
* Java字节码动态代理

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyInvocationHandler implements InvocationHandler {

    private Object target;

    public Object getProxy(Object target)
    {
        this.target = target;
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        return method.invoke(target, args);
    }
}

```

