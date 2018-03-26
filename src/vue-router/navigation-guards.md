# 导航守卫

`vue-router` 提供的导航守卫主要用来守卫路由，要么重定向导航，要么取消导航。在路由导航过程中有一些钩子：全局的、单个路由的或者组件内部的。

要记住，**params 或者 query 参数的改变不会处罚导航守卫的进入和离开**。你可以监听 `$route` 对象来应对这些变化，或者使用 `beforeRouteUpdate` 组件內的守卫。

## 全局守卫

你可以注册全局守卫，使用 `router.beforeEach`：

```js
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```

全局的 `before` 守卫会在导航触发时，依据创建顺序调用。守卫可以异步解决，当所有的钩子解决完之前，导航处于**暂停（pending）**状态。

每个守卫接收三个参数：

- `to: Route` 即将导航进入的目标 `Route` 对象。
- `from: Route` 当前的路由，即将离开
- `next: Function` 本函数必须调用才可以解决钩子（`resolve the hook`）。动作结果取决于传递给 `next` 的参数：
  - `next()` 前进到管道的下一个钩子。若无钩子剩余，导航就会被确定（confirmed）。
  - `next(false)` 中止当前的导航。如果浏览器的 URL 改变了（用户手动改变或者通过回退按钮），就会重置为 `from` 路由的地址。
  - `next('/')` 或 `next({ path: '/' })` 重定向到一个不同地址。当前的地址会被中断，并开启一个新地址。可以向 `next` 传递任何 location 对象。这允许你使用 `replace: true`, `name: 'home'` 或其他选项，就像在 `router-link` 的 `to` 属性或者 `router.push` 方法中使用的一样。
  - `next(error)` (2.4.0+) 如果参数是一个 Error 实例，导航中止，错误会当作参数传递到 `router.onError()` 中。

务必调用 `next` 函数，否则钩子无法解决。

## 全局 Resolve 守卫

> 2.5.0 新增

在 2.5.0+ 中，你可以注册一个全局守卫 `router.beforeResolve`。这个和 `router.beforeEach` 类似，区别在于 resolve 守卫会紧邻导航确认前被调用，跟随在所有的组件内守卫和异步路由组件被解决之后。

## 全局 After 钩子

你可以注册全局 after 钩子，但与守卫不同，这些钩子没有 `next` 函数，而且也无法影响导航：

```js
router.afterEach((to, from) => {
  // ...
})
```

## 路由特有守卫

可以在一个路由的配置对象中直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

这些守卫和全局 before 守卫的签名一样。

## 组件内守卫

最后，你可以在路由组件（传入到路由器配置的对象）内直接定义路由导航守卫，使用如下选项：

- `beforeRouteEnter`
- `beforeRouteUpdate` (2.2+ 新增)
- `beforeRouteLeave`

```js
const Foo = {
  template: '',
  beforeRouteEnter (to, from, next) {
    // 在渲染此组件的路由确认之前调用
    // 不可以使用 `this` 引用当前的实例
    // 因为执行该守卫时，组件尚未创建
  },
  beforeRouteUpdate (to, from, next) {
    // 
  },
}
```

（未完待续。。。）

## REF

- [Navigation Guards][nav-guard]

[nav-guard]: https://router.vuejs.org/en/advanced/navigation-guards.html