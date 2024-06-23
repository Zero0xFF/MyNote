#  Forest Blog分析笔记

通过web.xml定位前端控制器配置文( Spring配置文件)件的位置。

```
    <!--springmvc前端控制器 -->
    <servlet>
        <servlet-name>ForestBlog</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/spring-mvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>ForestBlog</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

查看spring配置文件，发现组件扫描标签。通过组件扫描标签，确定扫描的包。该标签的作用是让该包下的注解生效。。

```
<context:component-scan base-package="com.liuyanzhao.ssm.blog"/>
```

`<mvc:annotation-driven>`这个标签是用来解决映射器和适配器的注解配置，不需要关心，因为sping mvc项目一定包这个配置。

```
<!-- 一个配置节解决映射器和适配器的配置注解配置 -->
<mvc:annotation-driven></mvc:annotation-driven>

<!-- 如果不包含以上配置则需要手动添加 -->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

查看spring配置文件，找到视图解析器。确定jsp文件(视图文件)存放位置

```
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置前缀和后缀，也可以不指定-->
        <property name="prefix" value="/WEB-INF/view/"/>
        <property name="suffix" value=".jsp"/>
</bean>
```



