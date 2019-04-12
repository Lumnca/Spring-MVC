# :maple_leaf: Spring mvc创建项目

我们使用IDEA创建一个Spring Mvc项目，首先打开我们的编译器创建工程页：

首先选择Spring下的MVC，再加上下面的Java EE 的Web Appalachian，点击Configure按钮查看版本信息与配置。这个是自动把所需的包下载并添加到lib文件中，
完成之后点击next，如下：

![](https://github.com/Lumnca/Spring-MVC/blob/master/img/a1.png)

项目命名后再点击finsh,可以看到下载进度，完成后打开项目文件夹如下：


第一步要做的就是打开web.xml文件，查看配置信息：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.form</url-pattern>
    </servlet-mapping>
</web-app>
```

Web部署描述符web.xml是Java Web应用必不可少的配置文件。在这个文件中，你为你的应用程序定义servlet以及Web请求到这些Serlet的映射方式。
对于Spring MVC应用，你只需要定义一个Dispatcherservlet实例，作为Spring MVC的前端控制器，但是如果有必要，你可以定义超过一个。如上面的


```xml
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.form</url-pattern>
    </servlet-mapping>
```







