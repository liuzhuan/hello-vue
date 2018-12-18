# vuex

vuex 是官方提供的状态管理库，和 vue-devtools 集成，可以时间旅行调试（time travel debugging）。

首先看一下简单的 Vue 计数器应用：

```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },

  // view
  template: `
    <div>{{ count }}</div>
  `,

  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

Vuex 存储了应用所有组件的状态。组件树就变成一个巨大的 View。

下面是 Vuex 的一个图示。

![vuex diagram](https://vuex.vuejs.org/en/images/vuex.png)

Vuex 的数据无法直接变更，只能通过提交变更（**committing mutations**）改变状态。这保证了**每个状态变更都是可追溯的**。

## 安装

在浏览器使用 `https://unpkg.com/vuex` 即可使用最新的 Vuex 代码。

如果使用包管理工具，可以使用如下命令：

```sh
npm install vuex --save
# or
yarn add vuex
```

当使用模块系统，需要明确使用 `Vue.use()` 命令：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

## 最简单的 Store

创建 store 很简单，只需要提供一个初始的 state 对象和一些 mutation：

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

现在，就可以通过 `store.state` 获取状态对象，使用 `store.commit` 触发状态变更：

```js
store.commit('increment')
console.log(store.state.count) // -> 1
```

再次强调，我们通过提交 mutation 的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。

## 核心概念

### 状态 State

Vuex 使用**单一状态树**，这意味着每个应用只有一个状态。注意，单一状态树和模块化并不冲突，后面会看到如何将状态和变更拆分为子模块。

如何在组件中引入 Vuex 状态呢？

最简单的方法是通过计算属性：

```js
const Counter = {
  template: '<div>{{ count }}</div>',
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

每次当 `store.state.count` 变化时，会导致计算属性重新计算，触发相应的 DOM 更新。

但是，这种模式导致组件依赖全局 store 单例。Vuex 提供了一种机制，通过 `store` 选项，将 store “注入”根组件的所有子组件：

```js
const app = new Vue({
  el: '#app',
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

注入后，就可以通过 `this.$store` 访问。比如：

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

`mapState` 辅助函数

当组件需要多个 store 状态属性或者 getters 时，声明这些计算属性会有很多重复代码。此时，可以使用 `mapState` 帮我们节省精力：

```js
// 在完整构建版本中，辅助函数通过 Vuex.mapState 访问
import { mapState } from 'vuex'

export default {
  computed: mapState({
    count: state => state.count,
    countAlias: 'count',
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

也可以向 `mapState` 传递一个字符串数组：

```js
computed: mapState([
  // 将 store.state.count 映射为 this.count
  'count'
])
```

因为 `mapState` 返回一个对象。如果想要和其他本地计算属性综合使用，可以使用对象的扩展运算符（`...`）：

```js
computed: {
  localComputed () { /* ... */ },
  ...mapState({
    // ...
  })
}
```

### Getters 获取器

有时候需要根据 store 状态计算获取一些衍生数据，比如过滤列表元素并查询数量：

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果不只一个元素需要使用这个功能，就需要复制代码，或者创建辅助函数。这两种方法都不太理想。

为了解决这个问题，Vuex 为 store 提供了 Getters 函数，他们和组件的计算属性作用相似。

Getters 的第一个参数就是 state：

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

这些 Getters 可以通过 `store.getters` 像属性一样使用：

```js
store.getters.doneTodos
```

Getters 还可以接受其他 Getters，作为第二个属性：

```js
getters: {
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}

store.getters.doneTodoCount // -> 1
```

这样，就可以在任意组件轻松使用：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

也可以让 getter 返回一个函数，来实现给 getter 传参。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}

store.gettters.getTodoById(2)
```

`mapGetters` 辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter'
    ])
  }
}
```

如果你想将一个 getter 属性另取一个名字，使用对象形式：

```js
mapGetters({
  doneCount: 'doneTodosCount'
})
```

### Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的**事件类型 (type)**和一个**回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 increment 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 store.commit 方法：

```js
store.commit('increment')
```

提交载荷（Payload）

你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}

store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}

store.commit('increment', {
  amount: 10
})
```

对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 type 属性的对象：

```js
store.commit({
  type: 'increment',
  amount: 10
})

mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

Mutation 需遵守 Vue 的响应规则

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该
  - 使用 `Vue.set(obj, 'newProp', 123)`, 或者
  - 以新对象替换老对象。 `state.obj = { ...state.obj, newProp: 123 }`

### Action

### Module

## 项目结构

## 插件

## 严格模式

为了开启严格模式，只需要在创建 Vuex store 时传入 `strict: true` 即可：

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

严格模式下，每次使用非 mutation 钩子函数改变 Vuex 状态时，都会抛出异常。这就保证了所有的状态变更可以被调试工具显式跟踪。

## REF

- [State Management][state]
- [vuex on GitHub][github]
- [vuex][vuex]

[state]: https://vuejs.org/v2/guide/state-management.html
[github]: https://github.com/vuejs/vuex
[vuex]: https://vuex.vuejs.org/en/