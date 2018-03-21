# 命名路由

有时候使用命名路由会更方便，可以给 `routes` 提供一个名称：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

在 `<router-link>` 使用如下：

```html
<router-link :to="{ name: 'user', params: { userId: 123 } }">User</router-link>
```

和如下代码是相同的：

```js
router.push({ name: 'user', params: { userId: 123 } })
```

都会跳转到 `/user/123`。

## REF

- [Named Routes][named] - vuejs

[named]: https://router.vuejs.org/en/essentials/named-routes.html