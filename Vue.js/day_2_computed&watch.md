## 计算属性 `computed`
### Example
```
var vm = new Vue({
    el:"#app",
    data:{
        message:'Hello Vue!'
    },
    computed:{
        reverseMessage: function (){
            retrurn this.message.split('').reverse().join('')
        }
    }
})
```
> `computed` 为 `reverseMessage` 提供了 `getter` 方法，可以用 `vm.reverseMessage` 访问到。 

>同时它具有缓存机制，只有当与之关联的 `message` 发生变化时它才会被重新计算。

### `getter` & `setter`
> 除了`getter`方法， `computed` 也提供针对值的 `setter` 方法用于反向更新关联变量。
```
//...
computed:{
    fullName:{
        get: function() {
            return this.firstName + ' ' +this.lastName
        }
        set: function(newValue){
            names=newValue.split(' ')
            this.firstName=names[0]
            this.lastName=names[1]
        }
    }
}
//...
```


### `computed` vs `methods`
>用 `methods`属性可以实现同样的更新效果，但 `methods`每次访问 `reverseMessage`都会重新调用方法进行计算，开销大。

### `computed` vs `watch`
> `computed` 相当于 `watch` 的一个特例，它简化了 `watch` 对值进行更新的情况，因为这种情况很常用。 当 监控到变化后还需要执行其他逻辑代码时，使用更通用的 `watch`


## 侦听器 `watch`
>实现异步从后台请求数据，“实时”更新DOM

```
<div id="app">
    <p> Input your question end with '?'</p>
    <input v-model="question">
    <p>{{answer}}</p>
</div>
```

```
var vm = new Vue({
    el:'#app',
    data:{
        question:'',
        answer:''
    },
    watch:{
        question: function(newQuestin,oldQuestion){
            //doRelativeAjaxOrOthers...
        }
    }
})
``` 