# :fallen_leaf:Spring-MVC
Spring MVC框架学习，使用IDEA开发

SpringMVC是一种基于Java的实现MVC设计模型的请求驱动类型的轻量级**Web框架**，属于SpringFrameWork的后续产品，已经融合在Spring Web Flow中。

SpringMVC已经成为目前最主流的MVC框架之一，并且随着Spring3.0的发布，全面超越Struts2，成为最优秀的MVC框架。它通过一套注解，**让一个简单的Java类成为处理请求的控制器**，而无须实现任何接口。同时它还支持RESTful编程风格的请求。

## 什么是MVC？

我们可以对比Vue来看，MVVM模型，Vue在里面扮演着VM的角色。

再看MVC，M（Model）V（View）C（Controller），以Web项目为案例，我们可以看到前端需要后端给我们发送json数据或者进行页面跳转的控制，能够感受到SpringMVC其实就是起到了一个控制的作用，控制什么呢？控制视图（View）的转发，控制数据（Model）的处理。

## SpringMVC组件解析⭐⭐⭐⭐⭐

![](.\imgs\SpringMVC组件解析流程.png)

SpringMVC 的执行流程

1. 用户发送请求至前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器
3. 处理器映射器找到具体的处理器（可以根据xml配置、注解进行查找），生成处理器对象及处理器拦截器（如果有则生成）一并返回给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter处理器适配器
5. HandlerAdapter经过适配调用具体的处理器（Controller，也叫后端控制器）
6. Controller执行完成返回ModelAndView
7. HandlerAdapter将Controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中），DispatcherServlet响应用户

:maple_leaf: [Spring 介绍](https://github.com/Lumnca/Spring-MVC/blob/master/Spring%E5%AF%BC%E5%AD%A6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.md)

:maple_leaf: [Spring IOC](https://github.com/Lumnca/Spring-MVC/blob/master/IOC.md)

:maple_leaf: [Spring AOP](https://github.com/Lumnca/Spring-MVC/blob/master/AOP.md)

:maple_leaf: [Spring mvc项目创建](https://github.com/Lumnca/Spring-MVC/blob/master/hello.md)

:maple_leaf: [视图定位](https://github.com/Lumnca/Spring-MVC/blob/master/%E8%A7%86%E5%9B%BE%E5%AE%9A%E4%BD%8D.md)

:maple_leaf: [基于注解的控制器](https://github.com/Lumnca/Spring-MVC/blob/master/%E5%9F%BA%E4%BA%8E%E6%B3%A8%E8%A7%A3%E7%9A%84%E6%8E%A7%E5%88%B6%E5%99%A8.md)

:maple_leaf:[数据绑定和表单标签库](https://github.com/Lumnca/Spring-MVC/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A%E5%92%8C%E8%A1%A8%E5%8D%95%E5%BA%93%E6%A0%87%E7%AD%BE.md)

:maple_leaf:[转换器和格式化](https://github.com/Lumnca/Spring-MVC/blob/master/%E8%BD%AC%E6%8D%A2%E5%99%A8%E5%92%8C%E6%A0%BC%E5%BC%8F%E5%8C%96.md)

:maple_leaf:[验证器](https://github.com/Lumnca/Spring-MVC/blob/master/%E9%AA%8C%E8%AF%81%E5%99%A8.md)

:maple_leaf:[上传文件](https://github.com/Lumnca/Spring-MVC/blob/master/%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6.md)

:maple_leaf:[下载文件](https://github.com/Lumnca/Spring-MVC/blob/master/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6.md)
