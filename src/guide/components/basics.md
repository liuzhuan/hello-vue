# 组件

## 组件样例

```js
// Define a new component called button-counter
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

组件是拥有名字的可复用 Vue 实例，在本例中，`<button-counter>`。我们可以这样使用：

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
new Vue({ el: '#components-demo' })
```

由于组件是可复用的 Vue 实例，它们和 `new Vue` 的属性大体相同。唯一的不同是，组件没有根 Vue 实例特有的属性，比如 `el`。

## 复用组件

组件可以任意复用：

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

### `data` 必须是一个函数

之所以这样，是为了保证每个组件都有自己的 data 变量，防止组件互相污染。

## 通过 props 向子组件传递数据

props 是组件的自定义属性。

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

一旦 prop 注册成功，就可以向其传递参数：

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

在典型应用中，你的数据应该都储存在 data 中：

```js
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' },
    ]
  }
})
```

可以用循环渲染组件如下：

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```

## 单根元素

每个组件应该只保留一个根元素。

```html
<div class="blog-post">
  <h3>{{ post.title }}</h3>
  <div v-html="post.content"></div>
</div>
```

## 通过事件向父组件传递消息

为了向父组件发送事件，可以执行内置的 `$emit` 方法，传入事件的名称：

```html
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

然后，在组件中就可以使用 `v-on` 监听事件：

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

### 通过事件发送数值

有时候需要通过事件发送一些数据。可以使用 `$emit` 的第 2 个参数：

```html
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

当使用监听函数时，可以直接使用 `$event` 捕获该数据：

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

或者

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

```js
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

### 在组件上使用 `v-model`

```html
<input v-model="searchText">
```

相当于

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

当注册到自定义组件时，`v-model` 会变为这样：

```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

为了使该组件正常工作，组件内部的 `<input>` 必须：

- 将 `value` 属性绑定到 `value` prop
- 一旦监听到 `input`，抛出自己的 `input` 自定义事件，其中包含新值

代码如下：

```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```

这样，`v-model` 就可以和这个组件完美结合：

```html
<custom-input v-model="searchText"></custom-input>
```

## 通过插槽分发内容

```html
<alert-box>
  Something bad happened.
</alert-box>
```

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```

## 动态组件

使用 `<component>` 的 `is` 特别属性可以动态切换组件：

```html
<!-- Component changes when currentTabComponent changes -->
<component v-bind:is="currentTabComponent"></component>
```

在上面例子中，`currentTabComponent` 可以包含如下内容之一：

- 一个注册组件的名称，或者
- 一个组件的 options 对象

## DOM 模板解析警告

某些 HTML 元素，比如 `<ul>`，`<ol>`，`<table>` 和 `<select>` 对于其内部的子元素有限制，而一些元素比如 `<li>`，`<tr>`，和 `<option>` 只可以出现在某些元素中。

当使用这些元素时，这可能导致一些问题。比如：

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

此处的 `<blog-post-row>` 将会被误判为无效内容，导致最终渲染的页面错误。幸运的是，`is` 特殊属性提供了一种变通方法：

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

需要注意的是，这些限制对字符串模板无效：

- 字符模板（比如，`template: '...'`）
- 单文件组件（`.vue`）
- `<script type="text/x-template"/>`

## REF

- [Components - Vue.js][guide]

[guide]: https://vuejs.org/v2/guide/components.html