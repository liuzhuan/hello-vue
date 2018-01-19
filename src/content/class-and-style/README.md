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

## REF

- [Class and Style Bindings][guide]

[guide]: https://vuejs.org/v2/guide/class-and-style.html