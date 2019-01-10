## 列表渲染 `v-for`

### 遍历数组
```
<ul id="app-1">
    <li v-for="(item,index) in items">
        {{index}}. {{item.text}}
    </li>
</ul>
```
```
var app1 = new Vue({
    el:'app-1',
    data:{
        items:[
            {text:'first'},
            {text:'second'}
        ]
    }
})
```
> 第二个*para*：`index` 为可选参数。默认第一个参数为 *item in list*, 第二个参数为 *index if item*

### 遍历 `Object`
```
<ul id="app-2">
    <li v-for="(value,key,index) in item">
        {{index}}.{{key}}-{{value}}
    </li>
</ul>
```
```
//...
data:{
    item:{
        name: 'Grancis',
        age: '20',
        sex: 'man'
    }
}
//...
```
> 遍历单个`Object`时，`key`, `index` 为可选参数。

### `key`
> 对于已经被渲染出来的DOM元素，当与之关联的元素的`index`发生变化时，DOM元素将不会被重排。若要求DOM元素或Vue组件重排or重新渲染，需要为其提供`key`,用 `v-bind`进行绑定。
```
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```

## `list`的更新与检测

### 变异方法
>会改变原 `list`  的方法

#### `push()`
>在 `list` 尾部添加一个或多个元素

#### `pop()`
> 与`push()` 相反的方法，pop出`list`尾部的元素

#### `shift()` & `unshift()`
>操纵`list`头部元素，功能与`push()` & `pop()` 对应

#### `reverse()`
>使`list`倒序

#### `sort()`
>为指定参数时，按字母顺序(或转换为字母后)重新排序

>接受一个`function`作为参数， `function`需要返回 *小于0*、 *等于0*、 *大于0* 三种类型。[Details Here](http://www.w3school.com.cn/jsref/jsref_sort.asp)

#### `splice()`
>即`list`的切片功能[Detail Here](http://www.w3school.com.cn/jsref/jsref_splice.asp)

### 非变异方法
> 原`list`不发生变化，返回一个新的`list`

#### `slice()`
> 返回新`list`的切片方法

#### `filter()`
> 过滤方法，接受一个返回`Boolean`值的`function`

#### `concat()`
> 接受多个`list`,并拼接返回新`list`

### Vue无法响应式更新的`list`操作
```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```
> 对于第一个问题使用Vue提供的`set`方法解决

```
Vue.set(vm.items, indexOfItem, newValue)
```
或者使用全局方法 `Vue.set()` 的别名 `vm.$set()`(这是个实例方法)
```
vm.$set(vm.items, indexOfItem, newValue)
```

>对于第二个问题用前面提到的`splice()`方法
```
vm.items.splice(newLength)
```

## 响应式的对象更新

### 无法检测的更新方式
```
data:{
    item:{
        a:1,
    }
}
// vm.a 是响应式的
// vm.b=2 无法检测到
```
### 解决方法
>和`list`的类似，用全局方法`Vue.set(object, key, value)`或者`vm.$set(object, key, value)`

### 为对象增加多个属性
>不应该这样
```
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
>应该这样
```
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

## `v-for` 技巧

### `v-for` with `v-if` 
```
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

### `v-for` on a `template`
>利用带有`v-for`的`<template></template>`渲染多个元素
```
<ul>
    <template v-for="item in items">
        <li>{{item.msg}}
    </template>
</ul>
```

### `v-for` 在组件上的运用
>组件相当于自定义的一个`<template></template>`,注意事项为需要绑定组件提供的`prop`. 为实现解耦，不会自动绑定，需要手动绑定`prop`.例子在*day_1_about_introduction.md*中已经提到过.