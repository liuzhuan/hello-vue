## 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。

其实 HTML DOM 就是一种抽象语法树，Vue 的组件化系统可以让我们使用自定义标签。

Vue 组件是具备预定义参数的 Vue 实例。注册方式如下：

```js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```

然后就可以任意使用它：

```html
<ol>
  <todo-item></todo-item>
</ol>
```

还可以从父级向组件传递参数，通过 `props` 参数定义组件可以接收的参数即可：

```javascript
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

现在我们可以使用 `v-bind` 语法，向每个组件传递自定义的 `todo` 参数：

```html
<div id="app-7">
  <ol>
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```

```javascript
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Anything to eat' }
    ]
  }
})
```

使用组件可以让应用更易管理，分离主体框架和细节实现。下面是一个真实的组件样例：

```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

Vue 的组件化借鉴了 W3C 的 WebComponents 方案，但增加了很多实用扩展。

### REF

- https://cn.vuejs.org/v2/guide/index.html#组件化应用构建
- [WebComponents - W3C][component]

[component]: https://www.w3.org/wiki/WebComponents/