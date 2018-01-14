# Vue 实例

## 创建

Vue 系应用须以 `Vue` 函数开启：

```js
var vm = new Vue({
  // options
})
```

创建 Vue 实例后，可传入 options 选项。所有选项参数[见此][api]。

Vue 系应用包含一个 **root Vue** 实例，以 `new Vue` 创建。以及一些嵌套的，可复用的组件。比如，以下是一个 todo 应用的组件树示例：

```
Root Instance
+-- TodoList
  +-- TodoItem
  | +-- DeleteTodoButton
  | +-- EditTodoButton
  +-- TodoListFooter
    +-- ClearTodosButton
    +-- TodoListStatistics
```

Vue 组件亦为 Vue 实例，两者选项参数大部相同，仅有些许选项只可为 root Vue 实例可用。

## 数据和方法

创建 Vue 实例后，`data` 选项的所有属性均自动加入 Vue 的**响应式系统**。当这些数值变化时，对应的视图会随之刷新。

```js
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// These reference the same object!
vm.a === data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

## REF

- [The Vue Instance][instance]
- [Options - Data][api]

[instance]: https://vuejs.org/v2/guide/instance.html
[api]: https://vuejs.org/v2/api/#Options-Data