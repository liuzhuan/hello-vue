# 自定义指令 Custom Directives

[online reading](https://vuejs.org/v2/guide/custom-directive.html)

1. 自定义指令主要用来操纵底层 DOM 元素
2. 使用 `Vue.directive(id[, definition])` 注册全局指令
3. 使用 `directives` 字段注册局部指令
4. 指令定义包含如下 5 个钩子函数：
   1. `bind` 只调用一次，当指令第一次和元素绑定时调用
   2. `inserted` 当元素插入父节点时调用，不一定在文档中
   3. `update` 组件的 VNode 更新时调用，可能子节点尚未更新
   4. `comopnentUpdated` 子组件 Vnode 更新完毕后调用
   5. `unbind` 执行一次，当指令从元素中解绑时调用
5. 每个钩子函数的参数包含如下参数：
   1. `el` 指令绑定的元素
   2. `binding` 详见后面解释
   3. `vnode` Vue 编译器产生的虚拟节点
   4. `oldVnode` 上一个虚拟节点
6. `binding` 包含以下参数：
   1. `name` 指令名称，没有 `v-` 前缀
   2. `value` 指令的值
   3. `oldValue` 上一个值，仅在 `update` 和 `componentUpdated` 出现
   4. `expression` 绑定值的字面表达式
   5. `arg` 指令参数。`v-my-directives:foo` 中的 `foo`
   6. `modifiers` 包含修饰器的对象
7. 除 `el` 外，其他参数应当只读。如果需要在不同钩子函数中复用信息，可以使用元素的 `dataset` 属性
8. 当 `bind` 和 `update` 函数行为一样，不考虑其他钩子，可以使用函数的简写形式



```js
// 注册全局指令 `v-focus`
Vue.directive('focus', {
    inserted: function(el) {
        el.focus()
    }
})
```

```js
// 注册局部指令 `v-focus`
{
    directives: {
        focus: {
            inserted: function(el) {
                el.focus()
            }
        }
    }
}
```

```html
<!-- 使用指令 v-focus -->
<input v-focus>
```

函数的简写形式

```xml
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

```js
Vue.directive('demo', function(el, binding) {
    console.log(binding.value.color) // => 'white'
    console.log(binding.value.text) // hello!
})
```

