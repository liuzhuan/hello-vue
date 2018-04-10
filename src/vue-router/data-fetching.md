# 数据获取

有时候，某路由激活时需要从服务端拉取数据。比如，渲染用户资料页前，你需要从服务端拉取用户的数据。我们可以通过两种方式：

- **导航后拉取**：先导航，然后拉取数据。拉取过程中显示加载状态。
- **导航前拉取**：在路由 enter 守卫中拉取数据，然后导航

从技术上看，两者都可行 - 它最终取决于你想达到的用户体验。

## 导航后拉取

使用这种方法，马上导航并渲染新组件，然后在组件的 `created` 钩子函数中拉取数据。

比如，我们有一个 `Post` 组件，需要根据 `$route.params.id` 拉取帖子数据：

```html
<template>
  <div class="post">
    <div class="loading" v-if="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
```

```js
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },

  created () {
    this.fetchData()
  },

  watch: {
    '$route': 'fetchData'
  },

  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true

      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

## 导航前拉取

使用该方法，我们在跳转到新路由前拉取数据。我们可以在 `beforeRouteEnter` 中执行数据拉取动作，只有拉取结束后才执行 `next`：

```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```

## REF

- [Data Fetching][data-fetching]

[data-fetching]: https://router.vuejs.org/en/advanced/data-fetching.html