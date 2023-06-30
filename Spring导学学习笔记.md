# Spring导学

## 课程大纲

1. Spring概览
2. SpringIOC
3. Spring集成Web项目
4. SpringMVC
5. SpringAOP
6. Mybatis
7. SSM整合
8. 其他插件的整合
   1. Spring整合Junit（单元测试）

## Spring是什么？

Spring是分层的Java SE/EE应用full-stack**轻量级开源框架**，以**IOC**（Inverse of Control：控制反转）和**AOP**（Aspect Oriented Programming：面向切面编程）为内核。

提供了**展现层SpringMVC**和**持久层Spring JDBCTemplate**以及**业务层事务管理**等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE企业应用开源框架。

## Spring发展历程

1997年，IBM提出了EJB的思想

1998年，SUN制定开发标准规范 EJB 1.0

1999年，EJB 1.1 发布

2001年，EJB 2.0 发布

2003年，EJB 2.1 发布

2006年，EJB 3.0发布



**Rod Johnson（Spring 之父）**

Expert One-to-One J2EE Design and Development（2002）：阐述了 J2EE 使用 EJB 开发设计的优点及解决方案

Expert One-to-One J2EE Development without EJB（2004）：阐述了 J2EE 开发不使用 EJB 的解决方式（Spring 雏形）

2017年9月，发布了 Spring 的最新版本 Spring5.0 通用版（GA）

## Spring优势

**1）方便解耦，简化开发**

通过Spring提供的IoC容器，可以将对象间的依赖关系交由Spring进行控制，避免硬编码所造成的过度耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。

**2）AOP编程的支持**

通过Spring的AOP功能，方便进行面向切面编程，许多不容易用传统OOP实现的功能可以通过AOP轻松实现。

**3）声明式事务的支持**

可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务管理，提高开发效率和质量。

**4）方便程序的测试**

可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。

**5）方便集成各种优秀框架**

Spring对各种优秀框架（Structs、Hibernate、Hessian、Quartz等）的支持。

**6）降低 JavaEE API的使用难度**

Spring对JavaEE API（如JDBC、JavaMail、远程调用等）进行了薄薄的封装，使这些API的使用难度大为降低。

**7）Java 源码是经典学习范例**

## Spring体系结构

<img src=".\imgs\Spring体系结构图.png" style="zoom:50%;" />

### Core Container ⭐⭐⭐⭐⭐

Core Container即Spring的核心容器，是其他模块建立的基础，能够看到，类似于AOP、Data Access/Integration、Web模块都是建立在Core Container的基础上。Core Container由Beans模块、Core核心模块、Context上下文模块和Expression Language表达式语言模块组成，具体介绍如下：

备注：***Spring将管理对象称为Bean***

- **Beans模块**：提供了BeanFactory，是工厂模式的经典实现。
- **Core核心模块**：提供了Spring框架的基本组成部分，包括IOC和DI。
- **Context上下文模块**：建立在Core和Beans模块的基础之上，它是访问定义和配置任何对象的媒介。ApplicationContext接口是上下文模块的焦点。依赖包：spring-context
- Expression Language模块：是运行时查询和操作对象图的强大的表达式语言。

### 其他模块

Spring其他模块还有AOP、Aspects、Instrumentation以及Test模块，具体介绍如下：

- **AOP模块**：提供了面向切面编程实现，允许定义方法拦截器和切入点，将代码按照功能进行分离，以降低耦合性。
- Aspects模块：提供与AspectJ的集成，是一个功能强大且成熟的面向切面编程（AOP）模块。
- Instrumentation模块：提供了类工具的支持和类加载器的实现，可以在特定的应用服务器中使用。
- Test模块：支持Spring组件，使用JUnit或TestNG框架的测试。

### DATA Access/Integration（数据访问 / 集成）

数据访问/集成层包括JDBC、ORM、OXM、JMS和Transactions模块，具体介绍如下。

- JDBC模块：提供了一个JDBC的抽象层，大幅度减少了在开发过程中对数据库操作的编码。
- **ORM模块**：对流行的对象关系映射API，包括JPA、JDO、Hibernate和iBatis提供了继承层。
- OXM模块：提供了一个支持对象/XML映射的抽象层实现，如JAXB、Castor、XMLBeans、JiBX和XStream。
- JMS模块：指Java消息服务，包含的功能为生产和消费的信息。
- Transcations事务模块：支持编程和声明式事务管理实现特殊接口类，并为所有的POJO。

### Web模块

Spring的Web层包括Web、Servlet、Struts和Portlet组件，具体介绍如下。

- **Web模块**：提供了基本的Web开发集成特性，使用的Servlet监听器的IoC容器初始化以及Web应用上下文。
- Servlet模块：包括Spring模型—视图—控制器（MVC）实现Web应用程序。
- Struts模块：包含支持类内的Spring应用程序，集成了经典的Struts Web层。
- Portlet模块：提供了在Portlet环境中使用MVC实现，类似Web-Servlet模块的功能。

## Spring快速入门

主要展示Core Container模块为我们提供的功能，即**Context上下文模块**的功能：IOC。

1. 导入Spring开发的基本包坐标

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.20</version>
</dependency>
```

2. 编写Dao接口和实现类

```java
public interface UserDao {
    void say();
}

public class UserDaoImpl  implements UserDao {
    @Override
    public void say() {
        System.out.printf("Hello World！");
    }
}
```

3. 创建Spring核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

4. 在Spring配置文件中配置UserDaoImpl

```xml
<!-- Spring配置Bean的信息，然后通过Spring帮我们生成我们需要的Bean -->
<bean id="userDao" class="com.spring.quickstart.dao.impl.UserDaoImpl"></bean>
```

5. 使用Spring的API获得Bean实例

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao)app.getBean("userDao");
        userDao.say();
    }
}
```

