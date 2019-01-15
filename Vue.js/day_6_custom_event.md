# 自定义事件

## 通过自定义事件向父级组件传递信息

> 通过调整博客字号的例子来演示：

- 在父组件中添加`fontSize`这个`data`来支持字体大小的调整
  ```
  new Vue({
      el:"blog-enlarge-font-demo",
      data:{
          posts:[/*.../*],
          postFontSize: 1
      }
  })
  ```

- 模板中添加字体大小的`style`:
  ```
  <div id="blog-enlarge-font-demo">
    <div :style="{fontSize:postFontSize + 'em'}">
        <blog-post
        v-for="post in posts"
        v-bind="post">
        </blog-post>
    </div>
  </div>

- 为`<blog-post></blog-post>`组件增加一个放大字体的按钮：
    ```
    Vue.component('blog-post',{
        props:['post'],
        template:`
        <div class="blog-post">
            <h3 class="blog-title">{{post.title}}</h3>
            <div v-html="post.content"></div>
            <button>Enlarge Text</button>
        </div>
        `
    }) 
    ```
- 为`<button></button>`添加`click`事件时对应的`function`,但我们现在是想要这个`function`能够改变父组件的`postFontSize`，此时需要用到`$emit`,像父级元素抛出一个事件：
    ```
    <button @click="$emit('enlarge-text'，para)">Enlarge Text</button>
    ```
    > 现在点击`button`就会触发父级元素的`enlarge-text`事件，在父级组件对`enlarge-text`事件进行监控，即可在点击按钮时对`postFontSize`进行更新(由于`postFontSize`是父级组件的`data`，只能由父级组件进行更新，故需要这样的事件传递)。`para`为抛出事件时携带的参数，可选。

- 在父级元素添加`enlarge-text`事件的监控，并提供相应的相应`method`,上述`para`作为参数传递到`method`的参数上。
    ```
    <blog-post @enlarge-text="enlargeText">
    ```
    定义`method`
    ```
    methods:{
        enlargeText: function(enlargeAmount){}
    }
    ```
    若直接绑定`JavaScript`,可以用`$event`访问到被抛出的`para`:
    ```
    <blog-post @enlarge-text="postFontSize += $event">
    ```

>总结一下，子元素可以用`$emit('eventName',eventPara)`向父元素抛出一个自定义的事件，在父元素上可以向监听原生DOM事件一样用`v-on`对自定义事件进行监听，可以用`$event`访问抛出事件携带的参数。

## 事件名

> 监控的事件名称需要和抛出事件的名称完全匹配，而由于在DOM中使用`v-on`监控事件时，事件名会被完全转换为`lowercase`,所以使用大写字母的抛出事件将永远无法被监控到，故事件名推荐使用使用 `kebab-case`.

## `v-model` on *customer-component*

### `v-model`工作原理解析
使用`v-model`:
```
<input v-model="searchText">
```
等价于
```
<input
v-bind:value="searchText",
v-on:input="searchText=$event.target.value"
>
```

则在自定义组件中使用`v-model`时，也就相当于这样：
```
<customer-input
v-bind:value="searchText",
v-on:input="searchText=$event"
></customer-input>
```
为了让`<customer-input></customer-input>`有`value`需要为其定义这个`prop`,为了让其内部的`<input>`自组件发生输入时它能够监控到`input`事件并拿到输入值，需要在子组件`$emit`抛出一个`input`事件，如下：
```
Vue.component('customer-input',{
    prop:['value'],
    template:`
    /* ... */
    <input
    v-bind:value="value",
    v-on:input="$emit('input',$event.target.value)"
    >
    /* ... */
    `
})
```
然后就可以在自定义组件上使用`v-model`了：
```
<customer-input v-model="searchText"></customer-input>
```

> 嗯，大概是这样的(**划重点！！！**)。`v-bind`用于从`js`向DOM传递数据，`v-model`用于从DOM向`js`传递数据，实现的方法就是：通过`v-on`监控相应的DOM输入事件，并获取输入的值传给`js`中的`data`,而DOM中原本与输入相关联的`value`等属性再通过`v-bind`从`data`传入，保持了原本的DOM功能。

> 另外，`prop`用于从父级组件向子组件传递数据。大概就是这三种数据流。


### 更一般化的例子
`<input>`作为文本输入框时DOM中对应响应的是`value`，但如果是`<input type="ckeckbox">`时，响应的将是`checked`属性(这时`value`是一个不变的附带值)，此时`v-model`作用的组件要响应做出调整。

> 使用`model`选项注册组件：
```
Vue.component('base-checkbox''{
    model:{
        prop:'ckecked',
        event:'change'
    },
    props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
    `
})
```
此时可以使用`v-model`：
```
<base-ckeckbox v-model="lovingVue"></base-ckeckbox>
```
> 内部输入框的`checked`会传递到 `lovingVue` 上。


## 监控原生事件
>...待理解

## `.sync`修饰符
> ...待想理解
