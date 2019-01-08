# Vue.js 实现逻辑代码与操纵DOM的View操作分离
## View 层面组件化
```
<div id="app">
    {{message}}
</div>
```
## 逻辑代码中通过创建Vue实例与DOM组件进行绑定
```
var app = new Vue({
    el:'#app',
    data:{
        message:'Hello Vue'
    }
})
```
>在HTML中使用 `{{value}}`的方式引用变量，Vue的机制自动实现实例`app`里的`data`实时渲染到DOM中，从而只需要在逻辑代码中更新`data`中的`value`就能够对DOM进行更新。

>`app`实例中的 `el` `data`是Vue的保留关键字， `el` 用于将实例与对应的DOM绑定， `data`为实例的数据属性，此外还有`methods`为实例的方法。

# 绑定
## 变量绑定
>用`{{value}}`的语法进行变量绑定
## 属性绑定
```
<div id="app" v-bind:title="title">
    {{message}}
</div>
```
>用 `v-bind:`+属性 的语法绑定DOM对象的属性，上述div的title将随着实例中`data:{title:'my_title'}`的更新而自动更新
## 事件绑定
```
<div id="app" v-on:click:"clickEvent">
    {{message}}
</div>
```
>绑定事件的关键字为`v-on:`+ methodName, method在实例的methods属性中定义
```
var app = new Vue({
    el:'#app',
    data:{
        message:'Hello Vue'
    }
    methods:{
        clickEvent: function(){
            ;
        }
    }
})
```

# Vue定义的属性
## 循环 `v-for`
>渲染实例中list对象中的所有对象到DOM中(.html)
```
<div id="app-3">
    <ol>
        <li v-for="item in items>
            {{item.text}}
        </li>
    </ol>
</div>
```
```
var app = new Vue({
    el:'#app-3',
    data:{
        items:[
            {text:'item_1'},
            {text:'item_2'},
            {text:'item_3'}
        ]
    }
})
```

## 条件 `v-if`
>可以用于控制DOM对象的显示与否(.html)
```
<div id="app" v-if="seen">
    {{message}}
</div>
```
```
var app = new Vue({
    el:'#app-3',
    data:{
        seen:true
    }
})
```
>通过更新Boolean变量`seen`的值来控制DOM对象的显示与否

## 双向绑定 `v-model`
>实现从DOM对象到实例的数据流更新(.html)
```
<div id="app-4">
    <p>{{message}}</p>
    <input v-model="message">
</div>
```
>数据流：`v-model`=>`message`=>`<p></p>`

# 自定义组件
## 首先需要在Vue中注册组件(.js)
```
Vue.component('todo-item',{
    props:['todo'],
    template:'<li>{{todo.text}}</li>'
})
```
## 在DOM中可以嵌套在HTML标签中使用(.html)
```
<div id="app-5">
    <ol>
        <todo-item
        v-for="item in myList"
        v-bind:todo="item"
        v-bind:key="item.key>
        </todo-item>
    <ol>
</div>
```
## 创建`app-5`的Vue实例(.js)
```
var app2 = new Vue({
    el:'#app-5`,
    data:{
        myList:[
            {id:0,text:'ha'},
            {id:1,text:'haha'},
            {id:2,text:'hahaha}
        ]
    }
})
```
>在注册`<todo-item></todo-item>`时为其预留了`todo`属性，在其实例中将`todo`与`item`进行绑定，从而实现了`template`的渲染。

>`template:'<li>{{todo.text}}</li>'`相当于预渲染`{{todo.text}}`,当组件实例化时将实例变量`item`绑定到`todo`属性，完成真正想要的渲染。