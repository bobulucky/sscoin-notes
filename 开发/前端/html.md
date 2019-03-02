#### HTML,CSS,JavaScript
1. HTML是网页内容的载体。内容就是网页制作者放在页面上想要让用户浏览的信息，可以包含文字、图片、视频等。
2. CCS样式是表现。就像网页的外衣。比如，标题字体、颜色变化、或为标题加入背景图片、边框等。所有的这些用来改变内容外观的东西称之为表现。
3. JavaScript是用来实现网页上的特效效果。如：鼠标滑过弹出下拉菜单。或鼠标滑过表格的背景颜色改变。还有焦点新闻（新闻图片）的轮换。可以这么理解，有动画的，有交互的一般都是用JavaScript来实现的。   

##### HTML文件基本结构   
```
<html>
    <head>...</head>
    <body>...</body>
</html>
```

1. `<html></htm>`称为跟标签，所有网页标签都在`<html></html>`中。
2. `<head>`标签用于定义文档的头部，它是所有头部元素的容器。头部元素有`<title>`,`<script>`, `<style>`,`<link>`,`<meta>`等标签。
3. 在`<body>`和`</body>`标签之间的内容是网页的主要内容，如`<h1>`,`<p>`,`<a>`,`<img>`等网页内容标签，在这里的标签中的内容会在浏览器中显示出来。

##### <!DOCTYPE>声明   

`<!DOCTYPE>`声明有助于浏览器中正确显示网页。doctype不区分大小写。

##### 通用声明    

**HTML5**
`<!DOCTYPE html>`

**HTML4.01**
`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN""http://www.w3c.org/TR/html4/loose.dtd"`   

**XHTML 1.0**
`<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">`   

##### 中文乱码
大部分浏览器中，直接输出中文会出现中文乱码的情况，需要在头部将字符声明为UTF-8。
`<meta charset="UTF-8"/>`  

##### 标题
HTML标题是通过`<h1>-<h6>`标签来定义的。
```
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```
##### 段落
段落是通过`<p>`来定义的。
```
<p>这是第一段文字</p>
<p>这是第二段文字</p>
```
##### 链接
链接是通过标签`<a>`来定义的。
`<a href="http://www.github.com">GitHub网站链接</a>`
##### 图像
图像是通过标签`<img>`来定义的。
`<img src="img/timg.jpg"/>`

##### 属性
|属性|描述|
|---|---|
|class|为html元素定义一个或多个类名(classname)(类名从样式文件引入)|
|id|定义元素的唯一id|
|style|规定元素的行内样式(inline style)|
|title|描述了元素的额外信息(作为工具条使用)|

##### 文本格式化
html使用标签`<b>`("bold")与`<i>`("italic")对输出的文本进行格式化，如：**加粗** or *斜体*   
通常标签`strong`替换加粗标签`<b>`来使用，`<em>`替换`<i>`标签使用。

#### HTML文本格式化标签

|标签|描述|
|--|--|
|`<b>`|定义粗体|
|`<em>`|定义着重文字|
|`<i>`|定义斜体|
|`<small>`|定义小号字|
|`<strong`|定义加重语气|
|`<sub>`|定义下标字|
|`<sup>`|定义上标字|
|`<ins>`|定义插入字|
|`<del>`|定义删除字|

##### HTML 计算机输出 标签
|标签|描述|
|--|--|
|`<code>`|定义计算机代码|
|`<kbd>`|定义键盘码|
|`<samp>`|定义计算机代码样本|
|`<var>`|定义变量|
|`<pre>`|定义域格式文本|

##### HTML 引文，应用，及标签定义

|标签|描述|
|--|--|
|`<abbr>`|定义缩写|
|`<address>`|定义地址|
|`<bdo>`|定义文字方向|
|`<blockquote>`|定义长的引用|
|`<q>`|定义短的引用语|
|`<cite>`|定义引用、引证|
|`<dfn>`|定义一个定义项目|

#### HTML `<head>`元素
`<head>`元素包换了所有头部标签元素。插入脚本(script)，样式文件(CSS)，及各种meta信息。

可以添加在头部区域的元素标签为：`<title>`,`<style>`,`<meta>`,`<link>`,`<script>`，`<noscript>`,and `<base>`   

##### title元素
* 定义了浏览器工具栏的标题
* 当王爷添加到收藏夹时，显示在收藏夹中的标题
* 显示在搜索引擎结果页面的标题

##### base元素
标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接。
`<base href="http://www.sscoin.org/images/" target="_blank"` target是创建新的页面打开   

##### link元素
* 标签定义了文档与外部资源之间的关系
* 通常用于链接到样式表
`<link rel="stylesheet" type="text/css" href="mystyle.css"`  

##### style元素
* 定义html文档的样式文件引用地址。
* 在style元素中也可以直接添加样式来渲染html文档
```
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>
```

##### meta元素
* 描述了一些基本的元数据
* 提供了元数据，元数据不显示在页面上，但会被浏览器解析
* 指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。
* 元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他web服务。

为搜索引擎定义关键词：
`<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">`   

为网页定义描述内容：
`<meta name="description" content="免费 Web & 编程 教程">`   

定义网页作者：
`<meta name="author" content="SsCoin">`   

每30秒钟刷新当前页面：
`<meta http-equiv="refresh" content="30">`   

##### script元素
加载脚本文件   

#### HTML head元素
 |标签|描述|
 |--|--|
 |`<head>`|定义了文档的信息|
 |`<title>`|定义了文档的标题|
 |`<base>`|定义了页面链接的默认链接地址|
 |`<link>`|定义了一个文档和外部资源之间的关系|
 |`<meta>`|定义了HTML文档中的元数据|
 |`<script>`|定义了客户端的脚本文件|
 |`<style>`|定义了HTML文档的样式文件|


#### 样式-CSS
CSS(Cascading Styel Sheets)用于渲染HTML元素标签的样式。   
CSS是通过以下方式添加到HTML中：
* 内联样式---在HTML元素中使用"style"属性
* 内部样式表---在HTML文档头部head区域使用style元素来包含CSS
* 外部引用---使用外部CSS文件

#### 表格
```
<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>rows 1, cell 1</td>
        <td>rows 1, cell 2</td>
    </tr>
    <tr>
        <td>rows 2, cell 1</td>
        <td>rows 2, cell 2</td>
    </tr>
</table>
```   

#### 列表
无序列表：
```
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
    <li>item 4</li>
</ul>
```   
有序列表：
```
<ol>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
    <li>item 4</li>
</ol>
```   
自定义列表：
```
<dt>
    <dd>Coffee</dd>
    <dd>- black hot drink</dd>
    <dd>Milk</dd>
    <dd>- white cold drink</dd>
</dt>
```   
#### 区块
大多数的HTML元素被定义为**块级元素** 和**内联元素**   
* 块级元素：通常会以新行开始（和结束）`<h1>,<p>,<ul>,<table>`
* 内联元素：通常不会以新行开始 `<b>,<td>,<a>,<img>`   

#### div元素
div元素属于块级元素，他可以用于组合其他HTML元素的容器，没有特定含义。   

#### span元素
span元素是内联元素，可作用文本的容器，也没有特定含义。   

#### 表单
表单是一个包含表单元素的区域。   
表单元素是允许用户在表单中输入内容：文本域(textarea),下拉列表，单选框(radio-buttons)，复选框(checkboxes)等等。   
```
<!--文本域（Text Fields）-->
<form>
    First name:<input type="text" name="firstname"><br>
    Last name:<input type="text" name="lastname"><br>
</form>
<!--密码字段-->
<form>
    Password: <input type="password" name="pwd"><br>
</form>
<!--单选按钮（Radio Buttons）-->
<form>
    Male <input type="radio" name="sex" value="male"><br>
    Female <input type="radio" name="sex" value="female"><br>
</form>
<!--复选框（Checkboxes）-->
<form>
    <input type="checkbox" name="vehiche" value="bike">I have a bike<br>
    <input type="checkbox" name="vehiche" value="car">I have a car<br>
</form>
<!--提交按钮（Submit Button）-->
<form>
    <input type="submit" value="submit">
</form>
```   

#### 颜色
HTML颜色是一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成（RGB）。   
每种颜色的最小值是0（十六进制：#00）。最大值是255（十六进制#FF）。

#### URL-统一资源定位器
一个网页的地址实例：http://wwww.sscoin.org/hello/index.html 语法规则：   
> scheme://host.domain:port/path/filename   

* scheme - 定义因特网服务类型。最常见的类型是http
* host - 定义域主机（http的默认主机是www）
* domain - 定义因特网域名
* :post - 定义主机上的端口号
* path - 定义服务器上的路径
* filename - 定义文档/资源的名称   
