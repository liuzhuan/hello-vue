# Vue 实例的生命周期函数 lifecycle hooks

[online reading](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks)

1. 在生命周期函数中，`this` 总是指向 Vue 实例
2. 不要将生命周期定义为箭头函数，否则 `this` 指向会乱
3. 所有的生命周期函数依次如下：
   1. `beforeCreate` 初始化实例后立即执行，数据观察和事件/监听器设置前执行
   2. `created` 已设定数据观察、计算属性、函数和 watch/event 回调函数
   3. `beforeMounted`
   4. `mounted` 实例已被加载到页面，`el` 被 `$el.el`
   5. `beforeUpdate`
   6. `updated`
   7. `beforeDestroy`
   8. `destryoed`
   9. `activated` 当 `kept-alive` 组件激活时执行
   10. `deactived` 当 `kept-alive` 组件失活时执行
   11. `errorCaptured` 
4. `mounted` 不保证子组件全部挂载，可以使用 `vm.$nextTick` 等待全部子组件挂载
5. 服务端渲染不执行 `mounted` 钩子函数
6. `updated` 也不保证所有子组件渲染完成，可以使用 `vm.$nextTick` 等待



```js
{
    mounted: function() {
        this.$nextTick(function() {
            // 此代码，将在所有视图渲染后执行
        })
    }
}
```

```js
{
    updated: function() {
        this.$nextTick(function() {
            // 此代码，也在所有视图渲染后执行
        })
    }
}
```



## REF

- [API - Vue.js](https://vuejs.org/v2/api/#Options-Lifecycle-Hooks), Options/Lifecycle Hooks