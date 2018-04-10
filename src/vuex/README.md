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

Vuex 的数据无法直接变更，只能通过提交变更（**committing mutations**）改变状态。这保证了每个状态变更都是可追溯的。

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

创建 store 很简单，只需要提供一个初始的状态对象和一些变更函数即可：

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++
    }
  }
})
```

现在，就可以通过 `store.state` 获取状态对象，使用 `store.commit` 触发状态变更：

```js
store.commit('increment')
console.log(store.state.count)
```

## REF

- [State Management][state]
- [vuex on GitHub][github]
- [vuex][vuex]

[state]: https://vuejs.org/v2/guide/state-management.html
[github]: https://github.com/vuejs/vuex
[vuex]: https://vuex.vuejs.org/en/