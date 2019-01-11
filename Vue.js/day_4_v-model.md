## `v-model`  on `<input>`

### 文本 `type="text"`
```
<input v-model="message">
<p>{{message}}</p>
```
> 输入的*text*作`String`绑定到变量 `message`上

> 多行文本`<textarea></textarea>`用法一致

### 复选框 `type="checkbox"`
单个复选框将作为`Boolean`绑定到变量上
```
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
> `checked` 是个布尔变量

多个复选框，绑定到同一个数组上
```
<div id="example-1">
    <input type="ckeckbox" value="Jack" v-model="checkNames">
    <label>Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
</div>
```
> 也可以为`<input type="ckeckbox">` 指定 "选中"和"未选中"时传递的值，它不影响表单提交的`value`,只是另外指定了传个Vue实例的`data`

```
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```
```
// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```

> 配合`v-for`,可以实现动态渲染

```
<div id="example-2">
    <input tpye="checkbox" v-for="name in names" :value="name"  v-model="checkedNames">
</div>
```

### 单选按钮 `type="radio"`
> 将选中的`<input>`的`value`值传递到 `v-model`绑定的变量中

```
<div id="example-3">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```
```
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```

## `v-model` on `<select></select>`
总体上都可以与`<input>`进行类比

### 单选
```
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option value="a">A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```
> 传递选中的`value`,没有给`value`时传递内容。 如选中"A"时传递`"a"`,选中"B"时传递`"B"`

### 多选
> 传递到同一个数组上，不再赘述

### 动态渲染 `v-for` & `v-model`
```
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```

## 修饰符

- `.lazy`
- `.number`
- `.trim`

## `.lazy`
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：
```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

## `.number`
将用户的输入自动转换为数值类型
```
<input v-model.number="age" type="number">
```

## `.trim`
自动裁剪首位空白符
```
<input v-model.trim="message">
```