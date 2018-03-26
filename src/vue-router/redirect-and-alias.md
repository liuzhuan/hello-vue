# 重定向和别名

## 重定向

重定向可以在 `routes` 中配置。比如，将 `/a` 重定向至 `b`：

```js
const routes = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

重定向

## REF

- [Redirect and Alias][redirect-and-alias]

[redirect-and-alias]: https://router.vuejs.org/en/essentials/redirect-and-alias.html