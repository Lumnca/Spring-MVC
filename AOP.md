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

### AOP案例


