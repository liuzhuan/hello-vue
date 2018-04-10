# 路由元字段

定义路由时可以包括 `meta` 字段：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

如何使用 `meta` 字段呢？

首先，`routes` 配置中的每个路由对象称为**路由记录（`route record`）**。路由记录可以嵌套，因此当路由匹配到时，它可能匹配多于一个路由记录。

比如，对于上面的路由配置，URL `/foo/bar` 会同时匹配父路由记录和子路由记录。

所有匹配的路由暴露到 `$route` 对象（也会暴露到导航守卫的 `route` 对象）的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查元字段。

下面是一个全局导航守卫的用例，用来检查元字段：

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // 该路由需要授权，检查是否已经登入
    // 如果没有，重定向到登录页
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    // 记住一定要执行 next()
    next()
  }
})
```

## REF

- [Route Meta Fields][meta]

[meta]: https://router.vuejs.org/en/advanced/meta.html