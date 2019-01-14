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


