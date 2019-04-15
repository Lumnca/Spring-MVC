# :maple_leaf: Spring mvc创建项目

我们使用IDEA创建一个Spring Mvc项目，首先打开我们的编译器创建工程页：

首先选择Spring下的MVC，再加上下面的Java EE 的Web Appalachian，点击Configure按钮查看版本信息与配置。这个是自动把所需的包下载并添加到lib文件中，
完成之后点击next，如下：

![](https://github.com/Lumnca/Spring-MVC/blob/master/img/a1.png)

项目命名后再点击finsh,可以看到下载进度，完成后打开项目文件夹如下：

![](https://github.com/Lumnca/Spring-MVC/blob/master/img/a2.png)

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

这是一个前端控制器，代表着前台的请求应由谁来处理和应答。默认的是dispatcher，这是一个已经弄好了的控制器。需要修改的是它的` <url-pattern>*.form</url-pattern>`这是请求的域，我们设置为全局，修改为如下：

```xml
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```


接着就是我们dispatcher所对应的应交由什么servlet处理的xml文件修改，在WEB-INF下的dispatcher-servlet.xml中需要加入我们的所对应的控制器处理，到这里我们先写一个控制器代码，在src新建一个名为controller的包，再创建一个名为indexController的类，在里面编辑如下内容：

```java
package controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class indexController {
    @RequestMapping(value = "/index",method = RequestMethod.GET)
    public String Hello(ModelMap model){
        model.addAttribute("messgae","Hello World!");
        return "hello";
    }
}
```

关于代码解释以后再做介绍，这是一段向前台传输一段文字的方法，msg内容为Spring MVC Hello World。然后编辑上面所说的dispatcher-servlet.xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:component-scan base-package="controller"/>

    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <mvc:default-servlet-handler/>

    <mvc:annotation-driven/>
</beans>
```

如上面全部配置，若只换主体标签则不能使用其他标签，所以全换。其中对于各个标签的介绍如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

<context:component-scan base-package="controller"/>

<!-- 只把动态信息当做controller处理，忽略静态信息 -->
<mvc:default-servlet-handler/>

<!-- 开启注解 -->
<mvc:annotation-driven/>

<!--ViewResolver 视图解析器-->
<!--用于支持Servlet、JSP视图解析-->
<bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
    <property name="prefix" value="/WEB-INF/jsp/"/>           
    <property name="suffix" value=".jsp"/>
</bean>
</beans>
```

至此配置完毕，但是需要在WEB-INF下创建一个lib文件夹，并把外层的lib文件夹中的内容复制到其中，因为web项目运行需要包，但是默认的包还差了两个，即jstl.jar与standard.jsr。把这两个也一起添加进去，如果不行，则直接添加到out文件的web下的lib文件中。配置tomcat运行即可看到。如下是我的文件目录：

![](https://github.com/Lumnca/Spring-MVC/blob/master/img/a3.png)

上面是基于注解配置，也可以使用Controller接口完成，下面简单列出实现过程：

web.xml文件一样，不做多的修改。和注解的一样。主要是dispatcher-servlet.xml的修改：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean name="a" class="controller.IndexController"></bean>
        <bean id="simpleUrlHandlerMapping"
              class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
            <property name="mappings">
                <props>
                    <prop key="/success">a</prop>
                </props>
            </property>
        </bean>
</beans>
```

` <bean name="a" class="controller.IndexController"></bean>`定义了一个控制器类并指定到controller.IndexController，其名称为a。下面则是处理请求
发送请求所需要分配的控制器，`<prop key="/success">a</prop>`代表着请求为/success时所要交给的控制器名称的是a。a即是上面我们所定义的控制器名称。

接下来编写控制器类容：

```java
package controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class IndexController implements Controller {
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception
	{
		ModelAndView mav = new ModelAndView("index.jsp");
		mav.addObject("message", "Hello Spring MVC");
		return mav;
	}
}
```
 handleRequest(HttpServletRequest request, HttpServletResponse response)方法是接口中必须实现的方法，所以方法名不能做修改，并且参数也要为这个。
 ModelAndView（）构造方法接受一个字符串的参数，代表转到的路径。代表的一个视图的实例化。最后在index.jsp添加显示信息即可：
 
 ```
 <h4>${message}</h4>
  <form action="a">
    <input type="submit" value="转到">
  </form>
```


运行点击即可运行。

