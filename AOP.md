# AOP

### AOP核心概念

AOP(Aspect Oriented Programming)面向切面编程，一种编程范式，旨在将横切关注点（如日志记录、性能测量、安全性等）从主要业务逻辑中分离出来。它提供了一种将这些横切关注点模块化并集中处理的方式，而不是将它们分散到应用程序的各个部分中。

* 作用：在不影响原始设计的基础上为其进行功能增强，提高代码的模块性、可维护性和可重用性。通过将关注点从核心业务逻辑中分离出来，可以提供更清晰、更易于理解和维护的代码结构。AOP 还可以帮助减少代码重复，提高代码的可重用性。

假设我们现在要对操作进行记录：

```java
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("Save Book ...");
    }
    public void add() {
        System.out.println("2023/6/28 XXX add ...");
    }
    public void remove() {
        System.out.println("2023/6/28 XXX remove ...");
    }
    public void update() {
        System.out.println("2023/6/28 XXX update ...");
    }
}
```

可见每加入一个操作都要加一句打印语句，如果现在要修改打印语句内容，则需要对所有的打印语句进行修改，稍微显得麻烦。如果我们能够让程序知道调用哪些方法都需要做个记录，我们只需要写一个共同的方法让程序在调用这些方法的时候，再调用共同的方法，那就可以可以解决我们的问题了。
在以前的学习中我们可以用委托的方式来完成这个过程，如下:

```java
// 定义一个任务接口
interface Task {
    void execute();
}

// 实现具体的任务
class TaskImpl implements Task {
    @Override
    public void execute() {
        System.out.println("执行具体任务");
    }
}

// 委托方对象
class Delegator {
    private Task task;

    public Delegator(Task task) {
        this.task = task;
    }

    public void doTask() {
        task.execute();
    }
}

// 使用委托模式
public class Main {
    public static void main(String[] args) {
        Task task = new TaskImpl(); // 创建具体的任务对象
        Delegator delegator = new Delegator(task); // 创建委托方对象，并将任务委托给具体的任务对象
        delegator.doTask(); // 委托方对象执行任务
    }
}

```

当然对于Spring来说已经帮我们实现了这个流程，我们不用自己去实现，而实现这个过程的方法就是AOP

### AOP结构

AOP（面向切面编程）的结构包括以下几个核心组件：

* 1. 连接点（Join Point）：连接点是在应用程序执行期间可以插入切面的特定点。例如，方法调用是一个连接点，可以在该连接点处应用切面。在SpringAOP中可以理解为方法执行 （可以插入方法的地方）。
  
* 2. 切点（Pointcut）：切点是在应用程序中选择连接点的表达式。连接点表示在应用程序中可以插入切面的位置，如方法调用、方法执行、异常抛出等。切点通过表达式语言定义，以确定哪些连接点适用于给定的切面 （哪些地方需要插入方法）。

* 3. 通知（Advice）：通知定义了在切点处执行的具体操作(共同操作)。它是在切点周围织入的代码，用于实现横切关注点的行为。常见的通知类型包括前置通知（Before）、后置通知（After）、返回通知（After-returning）、异常通知（After-throwing）和环绕通知（Around）（插入的共同方法）。

* 4. 切面（Aspect）：切面是关注点的模块化。它定义了与特定关注点相关的行为。切面由切点和通知组成。切点指定在应用程序中哪些位置应该应用通知，而通知定义了在切点处执行的特定行为。


### AOP案例

导入jar包:

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
```

定义通知类（共性功能）

```java
public class Advice {
    
    public void record(){
        System.out.println("This is a record messgae");
    }
}
```

定义切入点:

```java
public class Advice {
    //切入点
    @Pointcut("execution(* org.example.dao.BookDao.*(..))")
    private void pt(){}

    //在哪里执行，切面
    @Before("pt()")
    public void record(){
        System.out.println("This is a record messgae");
    }
}
```

需要让Spring知道这是AOP通知类

```java
@Component
@Aspect
public class Advice {

    @Pointcut("execution(* org.example.dao.BookDao.*(..))")
    private void pt(){}

    @Before("pt()")
    public void record(){
        System.out.println("This is a record messgae");
    }
}

```

在Spring配置类或者启动类位置上加入`@EnableAspectJAutoProxy`注解

```java
@Configuration
@ComponentScan("org.example")
@EnableAspectJAutoProxy
public class SpringConfig {

}
```

在BookDao的实现类上标记为Bean：

```java
@Service
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("Save Book ...");
    }
    @Override
    public void add() {
        System.out.println("2023/6/28 XXX add ...");
    }
    @Override
    public void remove() {
        System.out.println("2023/6/28 XXX remove ...");
    }
    @Override
    public void update() {
        System.out.println("2023/6/28 XXX update ...");
    }
}
```
最后调用查看结果：

```java
public class Main {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        bookDao.add();
    }
}
```

输出以下内容说明配置成功！

```
This is a record messgae
2023/6/28 XXX add ...
```



