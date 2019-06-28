###JSP简介
JSP全称Java Server Pages，这是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。

###JSP结构
网络服务器需要一个JSP引擎，也就是一个容器来处理JSP页面。容器负责截获对JSP页面的请求。

###JSP处理
Web服务器是如何使用JSP来创建网页的：
* 浏览器发送一个HTTP请求给服务器
* Web服务器识别是对JSP网页的请求，并将该请求传递给JSP引擎
* JSP引擎从磁盘载入JSP页面，并将它们转换成Servlet
* JSP引擎将Servlet编译成可执行类，并将原始请求传递给Servlet引擎
* Web服务器调用Servlet引擎，然后载入并运行Servlet类
* Web服务器以静态HTML网页的形式将HTTP response返回到浏览器
* 最终，Web浏览器处理HTTP response中动态产生的HTML网页，就好像在处理静态网页一样

![Web服务器使用JSP创建网页](https://www.runoob.com/wp-content/uploads/2014/01/jsp-processing.jpg)

###生命周期
JSP生命周期走过的几个阶段：
* 编译阶段
  servlet容器编译servlet源文件，生成servlet类
* 初始化阶段
  加载JSP对应的servlet类，创建其实例，并调用它的初始化方法
* 执行阶段
  调用与JSP对应的servlet实例的服务方法
* 销毁阶段
  调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

![](https://www.runoob.com/wp-content/uploads/2014/01/jsp_life_cycle.jpg)

####JSP编译
当浏览器请求JSP页面时，JSP引擎会首先去检查是否需要编译这个文件。如果这个文件没有被编译过，或者在上次的编译后被更改过，则编译这个JSP文件。
编译过程的三个步骤：
* 解析JSP文件
* 将JSP文件转为servlet
* 编译servlet

####JSP初始化
容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法。

####JSP执行
这一阶段描述的是一切与请求相关的交互行为
当JSP网页完成初始化，JSP引擎将会调用_jspService()方法

####JSP清理
JSP生命周期的销毁阶段描述了当一个JSP网页从容器中被移除时所发生的一起

###语法
####脚本程序语法
```
<% 代码片段 %>
```

####JSP声明
一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用
```jsp
  <%! declaration; [declaration; ]+ ... %>
```

####JSP表达式
一个JSP表达式中包含的脚本语言表达式，先被转化成String，然后插入到表达式出现的地方

```
<%= 表达式 %>
```

####注释
不同情况下使用注释的语法规则：
语法|描述
--|--
<%-- 注释 --%>|JSP注释，注释内容不会被发送至浏览器甚至不会被编译
\<!-- 注释 -->| HTML注释，通过浏览器查看网页源代码时可以看见注释内容
<\\% |代表静态 <%常量
%\\>|代表静态 %> 常量
\\' |在属性中使用的单引号
\\"|在属性中使用的双引号

####JSP指令
JSP指令用来设置与整个JSP页面相关的属性
```
<%@ declaration attribute="value" %>
```
这里有三种指令标签：
指令|描述
--|--
<%@ page ...%>| 定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等
<%@ include ...%>|包含其他文件
<%@ taglib ...%>|引入标签库的定义，可以是自定义标签
