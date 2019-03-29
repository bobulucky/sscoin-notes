### 模板语法
Vue.js使用了基于HTML的模板语法。

#### 插值
----
##### 文本
数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值：   

```html
<span>Message: {{ msg }}</span>
```   

##### 原始HTML
双大括号会将数据解释为普通文本，输出真正的HTML，需要使用 ==v-html== 指令

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

##### 特性
Mustache 语法不能作用在HTML特性上，遇到这种情况应该使用
