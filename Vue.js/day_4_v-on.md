## 事件监控
> 使用 `v-on` 或者 缩写 `@event` 在DOM元素上添加事件监控，在触发事件时运行`JavaScript`代码。
```
<div id="example-1" >
    <button @click="count+=1">clicked {{count}} times</button>
</div>
```
```
var vm = new Vue({
    el:'example-1',
    data:{
        count:0
    }
})
```  
>当事件触发的逻辑代码更复杂时应该使用 `methods`，而不是直接内联`js`代码

## 调用`methods`时传参
> 方式与原生的`js`事件调用类似
```
<div id="example-3">
    <button @click="say('hi')">say hi</button>
    <button @click="say('hello')">say hello</button>
</div>
```

需要访问原始的DOM事件时，将`$event`作为参数传递即可
```
<button @click="warn('Form cannot be submitted yet.', $event)">Submit</button>
```
```
//...
methods:{
    warn: function(msg,event){
        if(event){
            event.preventDefault()
        }
        alert(msg)
    }
}
//...
```

## 事件修饰符
- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```
> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止所有的点击，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

>2.1.4 新增
```
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

>2.3.0 新增

Vue 还对应 `addEventListener` 中的 `passive` 选项提供了 `.passive` 修饰符。
```
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```
这个 `.passive` 修饰符尤其能够提升移动端的性能。

>不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。

## 按键修饰符 `v-on:keyup` or `@keyup`
for example:
```
<input @keyup.13="submit">
```
### 使用别名（ascii码不好记啊)
- .enter
- .tab
- .delete ('Delete' & 'Backspace` key)
- .esc
- .space
- .up
- .down
- .left
- .right

> 可以通过全局对象 `config.keyCodes` 进行按键修饰符别名自定义：
```
Vue.config.keyCodes.f1 = 112
//然后就可以使用 `@keyup.f1`
```

## 系统修饰符
> 2.1.0 新增

- .ctrl
- .alt
- .shift
- .meta

> `.meta`在Windows下为Windows微标键，macOS 上为 Command键

## `.exact` 修饰符

> 排除还有其他按键一起按下的情况，相当于“有且仅有”

```
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

## 鼠标按键修饰符

- .left
- .right
- .middle
  
> 这些修饰符会限制处理函数仅响应特定的鼠标按钮。

> 注意和按键监控区分：`@keyup.left` 系按键监控， `@click.left` 系鼠标监控

