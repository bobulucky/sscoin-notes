## IOC理论
IoC全称为==Inversion of Control==，翻译为“控制反转”，它还有一个别名为DI（==Dependency Injection==），即依赖注入。   
> **所谓IOC，就是由Spring IOC容器来管理对象的生命周期和对象之间的关系**   

为了更方便的理解“控制反转”，我们需要明白一下四个问题：   
1. 谁控制谁？---IoC容器控制了对象的创建
2. 控制什么？---对象被控制了
3. 为何反转？---以往是对象去创建现在变成IoC容器创建所以是反转了
4. 哪方面被反转了？---依赖对象的获取被反转了   

> **DI（依赖注入）其实就是IoC的另外一种说法，是由Martin Fowler在2004年初的一篇论文首次提出。==控制的什么被反转了？就是：获得依赖对象的方式被反转了==。**   

##各个组件
![](https://gitee.com/chenssy/blog-home/raw/master/image/201811/spring-201805091002.jpg)   
该图为ClassPathXmlApplicationContext的类继承体系结构(图片来源 http://singleant.iteye.com/blog/1177358)

##### Resource 体系   

##### BeanFactory 体系  

##### BeanDefinition 体系

##### BeanDefinitionReader 体系

##### ApplicationContext 体系


## Bean基础

三种方式注入值到bean属性：  
* 正常方式
```
<bean id="xxx" class="xxx">
    <property name="name">
        <value>Jack</value>
    </property>
</bean>
```
* 快捷方式   
```
<bean id="xxx" class="xxx">
    <property name="name" value="Jack"/>
</bean>
```
* “p” 模式
```
<bean id="xxx" class="xxx"
    p:name="name"/>
```   

Bean的作用域：   
* singleton: 单例，指一个Bean容器中只存在一份
* prototype: 原型，每次请求创建新的实例
* request: 请求，每次http请求创建一个实例且仅在当前request内有效
* session: 会话，每次http请求创建，当前session内有效
* global session: 全局会话，基于portlet的web中有效
