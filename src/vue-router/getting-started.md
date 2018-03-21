# 起步

vue-router 增加了两个组件 `router-link` 和 `router-view`。

HTML

```html
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <router-view></router-view>
</div>
```

JavaScript

```js
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar },
]

const router = new VueRouter({ routes })

const app = new Vue({
  el: '#app',
  router
})
```

注入路由器后，就可以使用 `this.$router` 获取路由器实例，并且使用 `this.$route` 获取当前的路由：

```js
// Home.vue
export default {
  computed: {
    username () {
      return this.$route.params.username
    }
  },
  methods: {
    goBack () {
      window.history.length > 1
        ? this.$router.go(-1)
        : this.$router.push('/')
    }
  }
}
```

当目标路由匹配时，`<router-link>` 会自动增加 `.router-link-active` 类。

## REF

- [Getting Started][start] - github

[start]: https://router.vuejs.org/en/essentials/nested-routes.html