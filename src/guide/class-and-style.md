# 绑定类和样式

一个常见的操作是操控一个元素的类和内联样式。由于它们都是属性，可以使用 `v-bind` 语法处理：只需计算出期望的字符串即可。但字符串拼接繁琐易错，因此 Vue 提供加强版 `v-bind`，特殊处理 `class` 和 `style`。使得表达式不仅可以处理字符串，还可以处理对象或数组。 

## 绑定 HTML 类

### 对象语法

可以向 `v-bind:class` 传递对象，动态切换元素类：

```html
<div v-bind:class="{ active: isActive }"></div>
```

如上语法表示，`active` 类存在与否，取决于 `isActive` 是否为真。

可以在对象中定义多个字段，也可以和普通 `class` 属性共存：

```html
<div class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

绑定的对象无需内联，写入 `data` 属性亦可。

```html
<div v-bind:class="classObject"></div>
```

```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

还可以使用计算属性返回的对象。这是一个常见的强力技巧：

```html
<div v-bind:class="classObject"></div>
```

```js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### 数组语法

可以向 `v-bind:class` 传入数组，增加多个对象：

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

将渲染为：

```html
<div class="active text-danger"></div>
```

在数组语法中，可以嵌套使用对象数组：

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### 同组件结合

当在自定义组件上使用 `class` 属性时，这些类会附加到组件的根元素上。该元素的现存类不会被覆盖。

比如，你有如下组件：

```js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

使用自定义组件时，增加一些类：

```html
<my-component class="baz boo"></my-component>
```

最终渲染的 HTML 变为：

```html
<p class="foo bar baz boo"></p>
```

类绑定语法也是如此：

```html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

若 `isActive` 为真值，最终渲染为：

```html
<p class="foo bar active"></p>
```

## 绑定内联样式

### 对象语法

To Be Continue...

## REF

- [Class and Style Bindings][guide]

[guide]: https://vuejs.org/v2/guide/class-and-style.html