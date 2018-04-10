# 过渡

由于 `<router-view>` 本质上是一个动态组件，我们可以使用 `<transition>` 组件施加同样的过渡效果：

```html
<transition>
  <router-view></router-view>
</transition>
```

## 每个路由过渡

上述用法会对所有路由施加同样的过渡效果。如果你想让每个路由组件拥有不同的过渡效果，你可以在每个路由组件中使用不同的过渡名称，来代替上面的`<transition>`：

```js
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

## 基于路由的动态过渡

还可以根据目标路由和当前路由的关系，动态决定使用什么过渡效果：

```html
<!-- 使用一个动态过渡名称 -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```

```js
// 然后在父组件中
// 监测 `$route` 变化，决定使用什么过渡效果

watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

## REF

- [Transitions][transitions]

[transitions]: https://router.vuejs.org/en/advanced/transitions.html