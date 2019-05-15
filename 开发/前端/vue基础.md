## 模板语法
Vue.js使用了基于HTML的模板语法。

### 插值
----
##### 文本
数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值：   

```html
<span>Message: {{ msg }}</span>
```   

##### 原始HTML
双大括号会将数据解释为普通文本，输出真正的HTML，需要使用 `v-html` 指令

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

##### 特性
Mustache 语法不能作用在HTML特性上，遇到这种情况应该使用v-bind指令：   
```html
<div v-bind:id="dynamicId"></div>
```

##### JavaScript表达式
对于所有的数据绑定，Vue.js都提供了完全的JavaScript表达式支持。
```html
{{ number + 1 }}
{{ ok ? 'YES' : 'NO'}}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```   

### 指令
----
指令(directives)是带有 `v-` 前缀的特殊特性。
指令特性的值预期是个单个JavaScript表达式。
指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式的作用域DOM。  
```html
<p v-if="seen">现在能看到我</p>
```
这里， `v-if` 指令将根据表达式 `seen` 的值的真假来插入/移除 `<p>` 元素。

##### 参数
一些指令能够接受一个参数，在指令名称后以冒号表示。
```html
<a v-bind:href="url">...</a>
```
这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` 特性与表达式 `url` 的值绑定。

##### 动态参数
> 2.6.0新增   

从2.6.0开始，可以用方括号括起来的JavaScript表达式作为一个指令的参数：
```html
<a v-bind:[attributeName]="url">...</a>
```
这里的 `attributeName` 会被作为一个JavaScript表达式进行动态求职，求得的值将会作为最终的参数来使用。  

**对动态参数的值的约束**   
动态参数预期会求出一个字符串，异常情况下值为==null==。

**对动态参数表达式的约束**   
动态参数表达式有一些语法约束，因为某些字符，例如空格和引号，放在HTML特性名里是无效的。同样，在DOM中使用模板时需要回避大写健名。  

##### 修饰符
修饰符(modifier)是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
```html
<form v-on:submit.prevent="onSubmit">...</form>
```


##### `v-bind` 缩写
```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```
##### `v-on` 缩写
```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```
