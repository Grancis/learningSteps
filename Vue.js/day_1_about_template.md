## Template的语法

### 插值 
#### `{{value}}` mustache 语法
```
<div id="app">
    <p>{{message}}</p>
</div>
```
>用mustache语法渲染的是纯文本

#### `v-html` 
```
<div id="app">
    <span v-html="rawHtml"></span>
</div>
```
>`<span v-html="rawHtml"></span>`相当于模板，渲染出来的是`rawHtml`中的HTML脚本

>注意防止xss攻击，只渲染信任的,可确定的`rawhtml`，不可渲染用户提供的

#### `v-bind` & `v-on` 
> `v-bind` 用于绑定 html DOM 对象的属性， `v-on` 绑定事件
- `v-on`修饰符：用 `.` 指明特殊后缀，如下：

```
<form v-on:submit.prevent="onSubmit">...</form>
```
>表明 `v-on` 对于出发的事件调用 `even.preventDefault()`
- 缩写
    - v-bind
    ```
    <a v-bind:href="myHref"></a>
    <--! 缩写 -->
    <a :href="myHref"></a>
    ```
    - v-on
    ```
    <button v-on:click="clickButton">点击</button>
    <--! 缩写 -->
    <button @click="clickButton">点击</button>
    ```



#### JavaScript表达式插值
>只能包含**单个表达式**,需要判断时使用*三元表达式*

`ok ? trueDo : falseDo`

>模板表达式无法访问用户定义的全局变量,只能访问Vue允许的全局变量白名单，如  `Math` `Date`