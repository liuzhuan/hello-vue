# 嵌套路由

真实的 app 界面通常由嵌套组件构成，常见的情况是 URL 的一个部分对应组件的一个嵌套部分。

使用 `vue-router` 很容易表达这种嵌套路由关系。

```html
<div id="app">
  <router-view></router-view>
</div>
```

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

（未完待续...）

## REF

- [Nested Routes][nested-routes] - vuejs

[nested-routes]: https://router.vuejs.org/en/essentials/nested-routes.html