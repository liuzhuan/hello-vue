# 条件渲染

## `v-if`

```html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

### 在 `<template>` 上使用 `v-if` 群组功能

`v-if` 是指令，需要依附在单个元素上。但是我们想控制一组元素，怎么办？我们可以把 `v-if` 放在 `<template>` 元素上，它可以作为看不见的包装。最终的渲染结果不包含 `<template>`。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

### `v-else`

`v-else` 元素需要紧紧跟随 `v-if` 或 `v-else-if` 元素，否则将失去意义。

### `v-else-if` （`v2.1.0+`）

同 `v-else` 一样，需要跟随有意义的元素后面。

### 通过 `key` 控制可再生元素

Vue 总会想法设法提高渲染效率，比如重用元素。这种方法不仅让 Vue 运行飞快，可能还有些其他好处。比如，你想让用户切换多种登录状态：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

当使用代码切换 `loginType` 时，用户输入的内容并不会消失。这是因为两个模板都用到了同一个元素 `<input>` ，切换时并没有重新生成它，只是改变了它的 `placeholder` 属性。

有时候这可能不是你想要的结果，因此聪明的 Vue 提供了一种方法，可以让你明确禁止重用元素。在元素上添加一个独一无二的 `key` 即可：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

## `v-show`

另一个切换元素可见性的指令是 `v-show`，用法大致与 `v-if` 相仿：

```html
<h1 v-show="ok">Hello!</h1>
```

区别在于 `v-show` 元素始终被渲染，只不过切换它的 `display` 属性而已。

> ⚠️ 注意：`v-show` 不支持 `<template>` 元素，也无法和 `v-else` 元素搭配。

## `v-if` vs `v-show`

`v-if` 是真正的条件渲染，因为它能确保条件变化时，语句块内的事件侦听器和子组件会被适当摧毁，或适当重建。

`v-if` 也很懒。如果第一次渲染时条件为假，它啥也不做。直到条件变成真，才会第一次渲染。

与之相对的，`v-show` 就比较简单多了，无论初始状态如何，元素始终会被渲染。后期只是切换 CSS 样式而已。

一般来讲，`v-if` 有更高的切换成本，而 `v-show` 有较高的初始化渲染成本。所以，如果你需要经常切换某元素，推荐使用 `v-show`。如果运行期间元素不会经常切换，推荐使用 `v-if`。

## `v-if` 和 `v-for`

当同时使用 `v-if` 和 `v-for` 时，`v-for` 优先级更高。具体可参见[列表渲染][list]。

## REF

- [Conditional Rendering - Vue.js][guide]

[guide]: https://vuejs.org/v2/guide/conditional.html
[list]: ./list.md