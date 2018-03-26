# 向路由组件传递参数

在组件中使用 `$route` 会在组件和路由间形成强耦合，这会限制组件的灵活性，因为它只能用在某些路由。

为了解耦组件和路由器，可以使用 `props` 选项：

**不要这样和 `$route` 耦合：**

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

**使用 `props` 将其解耦：**

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    /** 对于命名视图的路由，你需要为每个命名视图定义 `props` 选项 */
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

这可以让组件在任意地方使用，从而组件更容易复用和测试。

## 布尔模式

当 `props` 被设置为 `true`，`route.params` 将被设定为组件的属性。

## 对象模式

如果 `props` 是一个对象，这将直接作为组件属性。如果属性是静态值，这很有用：

```js
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})
```

## 函数模式

你可以创建一个返回属性的函数。这允许你将参数转换为其他类型，结合静态数值和路由参数等。

```js
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchBar, props: (route) => ({ query: route.query.q }) }
  ]
})
```

URL `/search?q=vue` 会把 `{query: 'vue'}` 作为参数传递给 `SearchUser` 组件。

尝试将 `props` 函数与状态无关，因为只有当路由变化时才会执行它。

## REF

- [Passing Props to Route Components][passing-props]

[passing-props]: https://router.vuejs.org/en/essentials/passing-props.html