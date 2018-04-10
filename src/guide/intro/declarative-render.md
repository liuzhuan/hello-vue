## 声明式渲染

Vue.js 的核心是一个允许采用简洁模板语法，来声明式的将数据渲染 DOM 的系统：

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

现在数据和 DOM 已经被绑定在一起，所有的元素都是**响应式**的。

除了文本插值，我们还可以采用这样的方式绑定 DOM 元素属性：

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

这里我们遇到点新东西。你看到的 `v-bind` 属性被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊属性。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。简言之，这里该指令的作用是：“将这个元素节点的 title 属性和 Vue 实例的 message 属性保持一致”。

### REF

- https://cn.vuejs.org/v2/guide/index.html#声明式渲染