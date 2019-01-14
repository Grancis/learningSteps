## 组件注册

### 组件vs实例
组件的注册于根实例的创建有很类似，也有不同之处
- 组件也是Vue的实例，是可复用的Vue实例，且带有名字
  ```
  Vue.componet('button-counter',{
      data: function(){
          return {
              count: 0
          }
      },
      template:'<buttton @click="count++">{{count}}</button>'
  })
  ```
  >组件和根实例一样，具有`data`, `computed`, `methods`, `watch` 和生命周期钩子等， 通过 `new Vue()`创建的根实例单独具有的是 `el`

  >由于组件是可以复用的实例，故其`data`需要用一个`funtion(){}`来返回，避免所有复用组件实例的`count`都是同一个变量的引用。
- 用`new Vue({})` 创建出来的的实例叫做 Vue 的跟实例，在根实例中可以将组件当做自定义元素来使用。
  ```
  <div id="example-1" :title="title">
    <button-counter></button-counter>
  </div>
  ```
  ```
  ex1 = new Vue({
      el:'#example-1',
      data:{
          title:'example'
      }
  }) 
  ```
  > `new Vue()` 创建的根实例，是指向DOM元素，真正使用来构建页面的元素，组件只有被根实例包含使用时才被渲染。

### 组件名

#### 防止冲突
遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)

#### 使用 kebab-case
```
Vue.componet('my-component',{})
```
#### 使用PascalCase
```
Vue.componet('MyComponent',{})
```
>使用组件时，`<MyComponent></MyComponent>` 和 `<my-component></my-component>`都可以使用，但直接在DOM中使用(即非字符串的模板) 中使用时只有 kebab-case 是有效的。原因是DOM不区分大小写

### 注册范围

#### 全局注册
即用`Vue.component()`的方法注册的组件，们在注册之后可以用在任何新创建的 Vue 根实例 (new Vue) 的模板中。比如：
```
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```
```
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```
> 在所有子组件中也是如此，也就是说这三个组件在各自内部也都可以相互使用。
#### 局部注册
始终使用全局注册，在最终构建系统时会包含所有组件（包括没使用到的），造成不必要`js`代码下载开销。
> 在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：
```
var componentA={/* ... */}
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```
> 使用时：
```
new Vue({
    el:'#app',
    components:{
        'component-a':componentA,
        'component-b':componentB,
        'component-c':componeetC
    }
})
```

### 模块系统(使用**webpack**打包)
/* 待学习 */


### 组件的复用
正如上述，组件可以复用，为了每一个复用组件的实例单独维护一份`data`，所以`data`必须是一个`function`
```
// in a componet
data: function () {
    return :{
        count ：0
    }
}
``` 

## `prop`

> 当组件被根实例作为子组件使用时，时常需要向子组件传递数据进行渲染，`prop`相当定义了组件的*属性*，为组件待渲染的数据提供赋值接口

```
Vue.componet('blog-post',{
    props:['title'],
    template:'<h3>{{title}}</h3>'
})
```
```
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

动态绑定：
```
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
```
```
<div id="blog-post-demo">
    <blog-post
    v-for="post in posts",
    :id="post.id",
    :title="post.title"
    ></blog-post>
</div>
```

### `prop`的大小写(camelCase vs kebab-case)
> 在HTML中不区分大写小，在注册时使用`camelCase`的，在DOM中使用时应转换为相应的`kebab-case`

```
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```
```
!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```
> 如果使用的是字符串模板则不存在这个限制。

### `prop`类型

> `props`可以指定数据类型,可以是任何类型。指定类型后，Vue会进行类型检查

```
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

#### `prop`验证 (使用到时查阅)
为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。
> 为了定制 prop 的验证方式，你可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：
 ```
 Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
 ```
 > 当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

 - 类型检查(`type`)
    >提供一个构造函数给Vue进行类型检查，可以是原生构造函数中的一个：
    - String
    - Number
    - Boolean
    - Array
    - Object
    - Date
    - Function
    - Symbol
    >也可以是自定义的构造函数：
    ```
    function Person(firstName,lastName){
        this.firstName=firstName,
        this.lastName=lastName
    }
    ```
    >then:
    ```
    Vue.component('blog-post',{
        props:{
            author:Person
        }
    })
    ```

### `prop`静态值与动态值的传递

- 静态值
  ```
  <blog-post title="My journey with Vue"></blog-post>
  ```
- 动态值,用`v-bind`
  ```
  <blog-post v-bind:title="post.title"></blog-post>
  <!-- 动态赋予一个复杂表达式的值 -->
  <blog-post v-bind:title="post.title + ' by ' + post.author.name"></blog-post>
  ```

> 静态值传递相当于告诉Vue要传递的是一个字符串，而用`v-bind`则告诉Vue我们要传递的是一个`javascript`表达式

### 传入数字
```
<!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:likes="42"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:likes="post.likes"></blog-post>
```

### 传入布尔值
> 默认为`true`

```
!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

### 传入数组
> 始终使用`v-bind`

```
<!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
```

### 传入一个对象
> 属于`js`代码，需要始终使用`v-bind`

### 传入一个对象的所有属性
> 若要传入一个对象的所有属性，用不带参数的`v-bind`代替`v-bind:prop-name`:

(会自动匹配？按照文档给的例子是这样的)

```
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```
以下模板：
```
<blog-post v-bind="post"></blog-post>
```
等价于：
```
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

### 单项数据流

> 看不大懂，大概就是.啥。这个`prop`有点像是在组件里用的另一种`v-model`

### 非`prop`的特性

> 可以理解为是DOM的特性吧，无法识别的特性就向外（根元素）方向抛出

例如，想象一下你通过一个 Bootstrap 插件使用了一个第三方的 `<bootstrap-date-input>` 组件，这个插件需要在其 `<input>` 上用到一个 data-date-picker 特性。我们可以将这个特性添加到你的组件实例上：
```
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```
然后这个 data-date-picker="activated" 特性就会自动添加到 `<bootstrap-date-input>` 的根元素上。

- 替换/合并 已有的特性
  >有些DOM特性在出现重复给出时会覆盖之前的，若`type="text"`会被后面的`type="date"`覆盖；而`class`和`style`则会合并多个特性。
  
  >外部提供给组件的特性与组件内部重复的特性遵循DOM的规则来处理。

- 禁用特性继承
  >.....看8懂
