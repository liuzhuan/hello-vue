# 列表渲染

## 使用 `v-for` 将数组映射为元素

我们可以使用 `v-for` 指令把一个数组映射为一系列元素。`v-for` 需要特殊语法 `item in items`，其中 `items` 是原始数组，而 `item` 是迭代过程中数组元素的代称。

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

在 `v-for` 代码块中，我们可以访问所有的父级属性。`v-for` 还支持可选的第二个属性，用作当前元素的索引值：

```html
<ul id="example-2">
  <li v-for=>
</ul>
```

To Be Continue...

## REF

- [List Rendering - Vue.js][guide]

[guide]: https://vuejs.org/v2/guide/list.html