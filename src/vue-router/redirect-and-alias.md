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

重定向还可以指向一个命名路由：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' } }
  ]
})
```

甚至使用函数实现动态重定向：

```js
const router = new VueRouter({
  routes: [
    { 
      path: '/a', 
      redirect: to => {
        // 函数接受目标路由当作参数
        // 返回重定向的路径
      } 
    }
  ]
})
```

注意，路由守卫不适用于重定向的路由，只适合目标路由。

## 别名

重定向指的是，当用户访问 `/a` 时，URL 会被替换为 `/b`，然后匹配为 `/b`。那什么是别名呢？

如果 `/a` 的别名是 `/b` 表示，当用户访问 `/b` 时，URL 依然保持为 `/b`，但是它会匹配为用户好像在访问 `/a` 一样。

上面的描述可以用代码表示为：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

## REF

- [Redirect and Alias][redirect-and-alias]

[redirect-and-alias]: https://router.vuejs.org/en/essentials/redirect-and-alias.html
[guard]: https://router.vuejs.org/en/advanced/navigation-guards.html