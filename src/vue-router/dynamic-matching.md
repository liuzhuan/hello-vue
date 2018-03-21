# 动态路由匹配

有时需要把多个路由映射到一个组件。比如我们有一个 `User` 组件，负责渲染每个用户，由于每个人都有不同 ID。在 `vue-router` 中，我们可以使用动态片段（`dynamic segment`）来达到这个目的：

```js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态片段以冒号开始
    { path: '/user/:id', component: User }
  ]
})
```

现在类似 `/usr/foo` 和 `/usr/bar` 的 URL 都会映射到同一个路由。

动态片段用 `:` 表示，当路由被匹配时，动态路由的参数会在每个组件中暴露为 `this.$route.params`。因此，我们可以通过更新 `User` 的模板来渲染当前的用户 ID：

```js
const User = {
  template: '<div>User {{ $router.params.id }}</div>'
}
```

## 应对参数变化

使用参数时，当地址从 `/usr/foo` 切换到 `/user/bar`，**会复用同一个组件**，这样会更有效率。但是，这也意味着**组件生命周期的钩子不会被调用**。

为了让组件能够监测到参数变化，你可以简单的监听 `$route` 参数：

```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // react to route changes...
    }
  }
}
```

或者，使用 2.2 引入的 `beforeRouteUpdate` 路由守卫：

```js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // 务必要调用 next()
  }
}
```

## 高级匹配模式

`vue-router` 使用 [path-to-regexp][path2reg] 作为路径匹配引擎，因此它支持许多高级匹配模式，比如可选的动态片段。更多高级特性可以查看它的文档。

## 匹配优先级

有时候一个 URL 可能会和多个路由匹配。此时，匹配优先级由路由表的定义顺序决定，位置越靠前，优先级越高。

## REF

- [Dynamic Route Matching][dynamic] - vuejs

[dynamic]: https://router.vuejs.org/en/essentials/nested-routes.html
[path2reg]: https://github.com/pillarjs/path-to-regexp