# 动态 & 异步组件

## 动态组件 `keep-alive`

我们使用 `is` 属性可以在 tab 页中切换组件

```html
<component v-bind:is="currentTabComponent"></component>
```

为了保持动态组件的状态，可以使用 `<keep-alive>` 元素将其包裹：

```html
<!-- Inactive components will be cached! -->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

## 异步组件

在大型项目中，我们可能需要将应用切分为小模块，每次当用到该模块时，才从服务端动态拉取。为了简化这个过程，Vue 允许你将组件定义为一个工厂函数，异步 resolve 组件定义。Vue 会在需要渲染该组件时触发该工厂函数，并将组件定义缓存，以便未来再次渲染使用。比如：

```js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // Pass the component definition to the resolve callback
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

工厂函数会接受一个 `resolve` 回调函数，当从服务端获取到组件定义后自动执行该回调函数。也可以执行 `reject(reason)` 来告知下载失败。如何获取组件取决于你，一种推荐的方法是综合使用 async 组件和 [Webpack 的代码分割][code-split]特性：

```js
Vue.component('async-webpack-example', function (resolve) {
  // This special require syntax will instruct Webpack to
  // automatically split your built code into bundles which
  // are loaded over Ajax requests.
  require(['./my-async-component'], resolve)
})
```

你还可以在工厂函数中返回一个 `Promise`，因此在 Webpack 2 和 ES2015 语法，可以这么写：

```js
Vue.component(
  'async-webpack-example',
  // The `import` function returns a Promise.
  () => import('./my-async-component')
)
```

当使用局部注册时，你可以直接提供一个返回 `Promise` 的函数：

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

### 处理加载状态

> 2.3.0+ 新增

异步组件工厂函数还可以返回如下对象：

```js
const AsyncComponent = () => ({
  // The component to load (should be a Promise)
  component: import('./MyComponent.vue'),
  // A component to use while the async component is loading
  loading: LoadingComponent,
  // A component to use if the load fails
  error: ErrorComponent,
  // Delay before showing the loading component. Default: 200ms.
  delay: 200,
  // The error component will be displayed if a timeout is
  // provided and exceeded. Default: Infinity.
  timeout: 3000
})
```

## REF

- [Dynamic & Async Components][guide]
- [Code Splitting - webpack][code-split]

[guide]: https://vuejs.org/v2/guide/components-dynamic-async.html
[code-split]: https://webpack.js.org/guides/code-splitting/