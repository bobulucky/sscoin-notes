#### CSS语法
CSS规则由两个主要的部分构成：选择器，以及一条或多条声明：
![](http://www.runoob.com/wp-content/uploads/2013/07/632877C9-2462-41D6-BD0E-F7317E4C42AC.jpg)  

#### id和class选择器
id选择器:  id="part1" -------> #part {}   
class选择器：class="center" -----> .center {}   

#### 创建CSS
插入样式表的方法有三种：
* 外部样式表
* 内部样式表
* 内联样式

```
<head>
<link rel="stylesheet" type="text/css" href="css/sheet.css">
</head>
```    
内部样式表：
```
<head>
<style>
hr {color:sienna;}
p {margin-left:20px;}
body {background-image:url("images.back40.gif");}
</style>
</head>
```  

内联样式：
`<p style="color:sienna;margin-left:20px;">段落</p>`   

#### 多重样式
如某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。  

优先级：
内联样式 > 内部样式 > 外部样式 > 浏览器默认样式
