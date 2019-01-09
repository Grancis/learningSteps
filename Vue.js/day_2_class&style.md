## 绑定HTML `class`
### *js*对象传递
```
<div v-bind:class="{active:isactive}"></div>
```
> `{active: isactive}` 是一个 *js* 对象，`active` 为 `String`, `isactive` 为 `Boolean`. 当 `isactive` 为 `true`时，渲染 `active`所代表的 `class`.

>也可以直接写对象的变量名
```
<div v-bind:class="clssObject"></div>
```
```
//for example
classObject={
    'isactive':true,
    'haseError':false
}
```
 >若`computed`返回的时符合要求的对象，也可以绑定 `computed` 对象。

>若组件模板中或DOM对象中带有`class`,那些`class`最终也会被渲染出来

 ### 数组传递
>相当于传递了一个`String`数组，每一个`String`都是一个`class`名
 ```
 <div v-bind:class="[activeClass,errorClass]"></div>
 ```
 ```
 data:{
     activeClass:'active',
     errorClass:'hasError'
 }
 ```

 > 数组中也可以包含*js*对象，`object`将按照*对象传递*的规则解析成`String`,仍是得到一个 `String` 数组
```
<div v-bind:class="[{'active':isActive},'hidden',classObject]"></div>
```


## `style` 绑定

### 对象传递
>传递一个满足 `css` 格式的对象
```
//e.g. 1
<div v-bind:style="{color:fontColor,font-size:fontSize}></div>
//e.g. 2 
<div v-bind:style="styleObject"></div>
```
```
data:{
    fontColor:'red',
    fontSize:'12px',
    styleObject:{
        color:'red',
        font-size:'12px'
    }
}
```
### 数组传递
>实际上时传递一个对象数组，包含的`object`满足上述要求
```
<div v-bind:style="[baseStyle,overrideStyle]"></div>
```
```
data:{
    baseStyle: {
        font-size:'12px',
        color:'red'
    }
}
```

