# Spring IOC

### 1.核心概念

### 2.入门案例

### 3.Bean

### 4.注解开发

***

## 核心概念

* IOC
* Bean
* DI

Spring框架的一个优点就是简化开发，我们看一下以前写代码的场景

数据层实现代码

```java
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("Save Book ...");
    }
}
```

表现层实现代码

```java
public class BookSericeImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();
    public void save() {
        bookDao.save()
    }
}
```

当我们需要换一个BookDao的实现类的时候，我们就需要重新修改：

```java
public class BookSericeImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl2(xx,xx,xx,xx,xx...);
    public void save() {
        bookDao.save()
    }
}
```

我们这里一但修改了源代码就需要重新编译，部署，发布。这些工作都是有成本的，导致这些最根本的原因呢其实就是我们在我们的类里面写了其他的实现，就导致了我们代码的耦合度较高，由于我们追求的程序的低耦合。所以我们想的就是降低代码耦合度。

```java
public class BookSericeImpl implements BookService {
    private BookDao bookDao;
    public void save() {
        bookDao.save()
    }
}
```

如上代码就只剩下一个接口了。但是我们这个代码肯定是运行不起来的，那该怎么办呢？

**解决方法**
* 使用对象时，在程序中不要主动使用new产生对象，转为外部提供对象。

## Ioc(Inversion of Control)控制反转 ##
* 对象的创建控制权由程序转移到了外部，这种思想称为控制反转。

这样做的好处是什么？其实就是为了**解耦** Spring和IOC有什么关系呢？其实Spring就是实现了IOC

**Spring技术对IOC思想进行了实现**

* Spring提供了一个容器，称为IOC容器，用来充当Ioc思想中的外部。

* IOC容器负责对象的创建和初始化等一系列工作，被创建或管理的对象在IOC容器中也称为Bean

那么当我们把对象的创建控制权由程序转移到了外部之后，我们的问题就要解决了吗？

```java
public class BookSericeImpl implements BookService {
    private BookDao bookDao;
    public void save() {
        bookDao.save()
    }
}
```

让我们再来看这个代码，虽然BookService这个实现类可以交给IOC容器来创建管理，但是这个类还需要一个BookDao的实现类，需要依赖于BookDao才能运行，那该怎么办呢？当然这个类也在IOC容器中，所以IOC会把这个两个类直接的依赖关系绑定上。

## DI（Dependency Injection）依赖注入

在容器中建立bean与bean之间的依赖关系的过程叫做依赖注入


目标：**充分解耦** 

* 使用IOC容器管理bean
* 在IOC容器内将有依赖的关系的bean进行关系绑定

效果

* 使用对象时不仅可以直接从IOC容器中获取，并且已经绑定了所有的依赖关系

***

## IOC入门案例

* 管理什么（Dao,Service）
* 如何将被管理的对象告知IOC容器(配置)
* 如何获取IOC容器（接口）
* 获取到容器后，又怎么获取bean呢？（接口方法）

**导入Spring包**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.23</version>
</dependency>
```

添加配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd"
>
</beans>
```

配置bean,告知那些类需要交给容器管理：

```xml
<bean id="bookdao" class="org.example.dao.impl.BookDaoImpl"/>
```

id 表示bean的名称（唯一） class指定类型

如何获取容器以及bean

```java
public class Main {
    public static void main(String[] args) {

        ApplicationContext ctx = new ClassPathXmlApplicationContext("abc.xml");
        BookDao bookDao = ctx.getBean("bookdao", BookDaoImpl.class);
        bookDao.save();
    }
}
```

## DI入门案例

* 我们的依赖关系是怎么注入的呢？（提供方法）
* 依赖关系之间如何描述（配置）

为了能够注入，为被注入类提供注入方法，提供对应的set方法:

```java
public class BookSericeImpl implements BookService {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    public void save() {
        bookDao.save()
    }
}

```

接下里就是描述依赖之间的关系：

```xml
<bean id="bookdao" class="org.example.dao.impl.BookDaoImpl"/>
<bean id="bookserver" class="org.example.service.impl.BookSericeImpl">
    <property name="bookDao" ref="bookdao"/>
</bean>
```

`property`表示配置属性其中name是属性名称，ref是参照引用类型

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("abc.xml");
        BookService bookService = ctx.getBean("bookserver", BookSericeImpl.class);
        bookService.show();
    }
}
```

***

### Bean

**Bean基础配置**

```xml
   <bean id="bookserver" name="b1 b2 b3" class="org.example.service.impl.BookSericeImpl" scope="prototype">
        <property name="bookDao" ref="bookdao"/>
    </bean>
```

`name` 别名可以取多个用空格隔开

`scope` 定义bean作用范围，可取值：
* singleton： 单例（默认）
* prototype： 多例

为什么要默认单例呢？

适合交给容器管理的bean：

* 表现层对象
* 业务层对象
* 数据层对象
* 工具对象

不适合的：
* 封装实体的域对象（有状态的成员变量值）

**bean的实例化**

IOC怎么创建bean的？

`反射机制`

```java
public class Main {
    public static void main(String[] args) {
        try {
            Class<?> aClass = Class.forName("org.example.service.impl.BookSericeImpl");
            Constructor<?>[] c = aClass.getConstructors();
            try {
                BookService bookService = (BookService) c[0].newInstance();
                bookService.save();
            }
            catch (Exception e){
                System.out.println(e.getMessage());
            }
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
}
```

IOC默认调用无参构造方法。这就是实例化第一种方式--构造方式

```xml
<bean id="bookdao" class="org.example.dao.impl.BookDaoImpl"/>
```



**静态工厂实例化（了解）**

早些时候常用一种叫做静态工厂方式示例化对象：

```java
public class BookFactory {
    public static BookSericeImpl getBookServer(){
        return  new BookSericeImpl();
    }
}
```

我们也可以配置用静态工厂实现bean的示例化：

```xml
<bean id="factory" class="org.example.factory.BookFactory" factory-method="getBookServer"/>
```

`factory-method`指定生成对象的方法。

**实例工厂实例化**

与静态工厂实例化不一样的地方就是改为实例方法。

```java
public class BookFactory {
    public BookSericeImpl getBookServer(){
        return  new BookSericeImpl();
    }
}
```

那么对应配置的第一步就应该是配置FactoryBean：

```xml
<bean id="factory" class="org.example.factory.BookFactory"/>
```

第二部就是指定实例工厂实例化bean:

```xml
<bean id="bookserver" name="b1 b2 b3" class="org.example.service.impl.BookSericeImpl" factory-bean="factory" factory-method="getBookServer">
    <property name="bookDao" ref="bookdao"/>
</bean>
```

第一个bean是为了第二个生成的bean而创建的实际上意义不大。Spring提供一种专门实现工厂类的接口FactoryBean，使用这个接口就无需配置实例工厂bean


```java
public class BookFactory implements FactoryBean<BookService> {

    //代替原始实例工厂中创建对象的方法
    @Override
    public BookService getObject() throws Exception {
        return new BookSericeImpl();
    }

    //指定生成bean的类型
    @Override
    public Class<?> getObjectType() {
        return BookService.class  
    }
    //指定作用范围
    @Override
    public boolean isSingleton() {
        return FactoryBean.super.isSingleton();
    }
}
```

指定生成方式：

```java
 <bean id="bookserver" name="b1 b2 b3" class="org.example.factory.BookFactory"/>
```

**bean的生命周期**

* bean生命周期：bean从创建到消亡的完整过程
* bean生命周期控制：在bean创建后到销毁前做一些事情

Bean生命周期：

* 初始化容器
    * 1. 创建对象（分配内存）
    * 2. 执行构造方法
    * 3. 执行属性注入
    * 4. 执行bean初始化方法
* 使用bean
    * 执行业务操作
* 关闭/销毁容器
    * 执行bean的销毁方法

```xml
<bean id="bookserver"  class="org.example.service.impl.BookSericeImpl"  init-method="init" destroy-method="destroy"/>
```

`init-method`指定创建后执行方法

`destroy-method`bean销毁之前执行方法

我们可以关闭容器看到destroy方法执行。但是对于`ApplicationContext`容器接口是没有这个方法的。想要关闭容器可以使用`ClassPathXmlApplicationContext`实现类声明容器

```java
public class Main {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("abc.xml");
        BookService bookService = ctx.getBean("bookserver",BookService.class);
        bookService.show();
        ctx.close();
    }
}
```

当然也可以使用`ctx.registerShutdownHook();`设置容器关闭钩子。对生命周期控制配置还有统一的实现规范，就不用在bean中配置对应方法：

```java
public class BookFactory implements InitializingBean, DisposableBean {
    
    @Override
    public void destroy() throws Exception {
        System.out.println(" destroy");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("init");
    }
}
```

**依赖注入方式**

向一个类中传递数据的方式有几种？
* 普通方法（set方法）
* 构造方法

前面介绍的都是引用类型，那么对于简单类型又是怎么做到的呢？

依赖注入方式：

* setter注入（引用，简单类型）

```xml
<bean id="bookserver" name="b1 b2 b3" class="org.example.factory.BookFactory" autowire="byName" >
    <property name="v1" value="100"/>
    <property name="v2" value="abc"/>
</bean>
```

注意注入类需要有属性的可访问的set方法。


* 构造器注入 （引用，简单类型）

首先含有构造类：

```java
    public BookSericeImpl(BookDao bookDao){
        this.bookDao = bookDao;
    }
```

再做配置：

```xml
<bean id="bookserver"  class="org.example.service.impl.BookSericeImpl" >
    <constructor-arg name="bookDao" ref="bookDao"></constructor-arg>
</bean>
```

依赖注入方式选择

* 强制依赖使用构造器进行，使用setter注入有概率没有注入成功
* 可选以来使用setter注入进行，灵活性强
* Spring框架倡导使用构造器，第三方框架大多数都是采用这种相对严谨
* 自己开发的模块推荐使用setter注入



### 自动装配


```xml
<bean id="bookserver"  class="org.example.service.impl.BookSericeImpl" autowire="byType"/>
```

`autowire` 自动装配类型：按照类型，按照名称装配

```xml
<bean id="bookDao" class="org.example.dao.impl.BookDaoImpl"/>

<bean id="bookserver" class="org.example.service.impl.BookSericeImpl" autowire="byName"/>
```

注意事项
* 自动装配用于引用类型依赖注入，不能对简单类型进行操作
* 使用按类型装配时候必须保障容器中相同类型bean唯一
* 按名称装配时候必须保证容器中具有指定属性名称的bean，因为变量名与配置耦合，不推荐使用。
* 自动配置优先级低于setter注入与构造器注入，同时出现自动装配失效。


## 注解开发

对于前面的操作开发，均需要在配置文件配置，使用起来还是不够方便，Spring提供了注解开发，我们可以使用启动配置类来替换配置文件

```java
@Configuration
@ComponentScan("org.example")
public class SpringConfig {

}
```

对于原来的Bean配置只需要在原始的类上加上`@Component`注解即可表示向容器中注册一个Bean。

```java
@Component
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("Save Book ...");
    }
}
```

对于修改Bean为多例可以在类的前面写上`@Scope("prototype")`注解。

对于依赖注入可以使用`@Autowired`注解注入对象，@Value注入值类型：

```java
@Service
public class BookSericeImpl implements BookService {

    @Autowired
    BookDaoImpl bookDao;

    @Value("4")
    private int id;
    
    @Value("aaa")
    private String name;

    @Override
    public String show() {
        bookDao.save();
        return "Book Name is "+name+" Id:"+id;
    }
}

```

注意为了区分Bean类型还提供了以下三个注解都和`@Component`一样的作用只是用来区分功能：

`@Controller` Web应用程序控制器
`@Service` 处理业务逻辑和实现各种操作
`@Repository` 数据访问的组件，通常与数据库交互。

最后修改读取文件为启动类即可：

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookSericeImpl bookSerice = ctx.getBean(BookSericeImpl.class);
        bookSerice.show();

    }
}
```

配置文件值读入值注入：可以通过注解`@PropertySource("jdbc.properties")来读取配置文件，多个使用{"xx","xxx"}引用。在需要注入的地方使用$

配置文件jdbc.properties:

```
id=6
name=bbb
```

加入配置文件

```java
@Configuration
@ComponentScan("org.example")
@PropertySource({"jdbc.properties"})
public class SpringConfig {

}
```

配置文件注入：

```java
@Service
public class BookSericeImpl implements BookService {

    @Autowired
    BookDaoImpl bookDao;

    @Value("${id}")
    private int id;
    @Value("${name}")
    private String name;

    @Override
    public String show() {
        bookDao.save();
        return "Book Name is "+name+" Id:"+id;
    }
}

```






