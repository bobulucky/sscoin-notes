JavaEE体系结构包括四层，从上到下分别是应用层、Web层、业务层、持久层。Struts和SpringMVC是Web层的框架，Spring是业务层的框架，Hibernate和MyBatis是持久层的框架。

在早期Java Web的开发中，统一把显示层、控制层、数据层的操作全部交由JSP或者JavaBean来进行处理，我们称之为Model1：   
![](https://upload-images.jianshu.io/upload_images/7896890-7b3f9cd59394b017.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   
出现的弊端：
* JSP和JavaBean之间严重耦合，Java代码和HTML代码也耦合在一起
* 要求开发者不仅要掌握Java，还要有高超的前段水平
* 前段和后端互相依赖，前端需要等待后端完成，后端也依赖前端完成，才能进行有效的测试
* 代码难以复用

很快这种方式被Servlet + JSP + JavaBean 所替代，早起的MVC模型Model2：
![](https://upload-images.jianshu.io/upload_images/7896890-403a273b08fec826.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

用户的请求会到达Servlet，然后根据请求调用相应的JavaBean，并把所有的显示结果交给JSP去完成，这样的模式我们就称之为MVC模式。
* M 代表 模型(Model)
  模型是什么？ 模型就是数据，就是dao，bean。
* V 代表 视图(View)
  视图是什么？ 就是网页，JSP，用来展示模型中的数据。
* C 代表 控制器(controller)
  控制器是什么？ 控制器的作用就是把不同的数据(Model)，显示在不同的视图(View)上，Servlet扮演的就是这样的角色。

#### MVC设计模式
MVC设计模式的任务是将包含业务数据的模块与显示模块的视图解耦。

如何实现此任务？

只需在模型和视图之间引入重定向层可以完成次任务。此重定向层就是控制器，控制器接收请求，执行更新模型的操作，然后通知视图关于模型更改的消息。

![](https://i.imgur.com/XApjGKM.png)   

![](https://i.imgur.com/iEQSPKx.png)   

#### 为什么要使用SpringMVC？
应用程序的问题：处理业务数据的对象和显示业务数据的视图之间存在紧密的耦合。

SpringMVC是一种基于Java,实现了Web MVC设计模式，请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将Web层进行职责解耦。

为解决持久层中一直未处理好的数据库事务的编程，又为了迎合NoSQL的强势崛起，Spring MVC给出了方案：   
![](https://upload-images.jianshu.io/upload_images/7896890-a25782fb05f315de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   
传统的模型层被拆分为了业务层(Service)和数据访问层(DAO,Data Access Object)。在Service下可以通过Spring的声明式事务操作数据访问层，而在业务层上还允许我们访问NoSQL，这样就能满足异军突起的NoSQL的使用了，它可以大大提高互联网系统的性能。

**特点**
* 结构松散，几乎可以再Spring MVC中使用各类视图
* 低耦合，各个模块分离
* 与Spring无缝集成

**Spring MVC是Spring的一部分**：   

![](https://i.imgur.com/Qm3U8Fw.png)   

**Spring MVC的核心架构：**

![](https://i.imgur.com/DT9IY0g.png)   

![](https://upload-images.jianshu.io/upload_images/7896890-65ef874ad7da59a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

**具体流程：**   
1. 首先用户发送请求------>DispatcherServlet，前段控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制。   
2. DispatcherServlet------>HandlerMapping，处理器映射器将会把请求映射为HandlerExecutionChain对象(包含一个Handler处理器对象、多个HandlerInterceptor拦截器)对象。
3. DispatcherServlet------->HandlerAdapter，处理器适配器将会把处理器包装成为是适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器。   
4. HandlerAdapter------>调用处理器相应功能处理方法，并返回一个ModelAndView对象(包含模型数据、逻辑视图名);
5. ModelAndView对象(Model部分是业务对象返回的数据模型，View部分为逻辑视图名)------>ViewResolver,视图解析器将把逻辑视图名解析为具体的View;   
6. View------>渲染，View会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构；
7. 返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到此一个流程结束。   
