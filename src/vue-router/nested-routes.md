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

此处的 `<router-view>` 是一个顶级出口，它匹配顶级路由。同样的，组件也可以包含自己的 `<router-view>`，比如，在 `User` 组件中添加如下模板：

```js
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```

为了能在这个内嵌的出口渲染组件，需要在 `VueRouter` 构造函数的配置项中增加 `children` 选项：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        // 匹配 `/user/:id/profile`
        { path: 'profile', component: UserProfile },
        // 匹配 `/user/:id/posts`
        { path: 'posts', component: UserPosts }
      ]
    }
  ]
})
```

注意，以 `/` 开头的嵌套路径会被当作根路径。

此时，如果访问 `/user/foo`，可能不会有任何内容渲染，因为没有找到匹配的子路由。如果你确实想要展示一些内容，此时你可以提供一个空白子路由：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        { path: '', component: UserHome }
        // ... other sub routes
      ]
    }
  ]
})
```

## REF

- [Nested Routes][nested-routes] - vuejs

[nested-routes]: https://router.vuejs.org/en/essentials/nested-routes.html