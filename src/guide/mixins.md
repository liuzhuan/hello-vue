# 混入 Mixins

[online reading](https://vuejs.org/v2/guide/mixins.html)

1. 当组件使用混入时，混入的字段将混入组件中。
2. 当混入和组件包含相同选项时，会根据不同策略合并。
3. data 数据将使用递归合并策略，组件的优先级更高。
4. 钩子函数将组成一个数组，混入的函数先执行，组件的函数后执行。
5. `methods`, `components`, `directives` 将合并为一个对象。组件的数据优先级更高。
6. 还可以使用全局混入。但要注意，全局混入不仅影响自己的应用代码，还会影响第三方 Vue 框架。

## Code Example

```js
var myMixin = {
    created: function() {
        this.hello()
    },
    methods: {
        hello: function() {
	    console.log('hello from mixins!')
	}
    }
}

var Component = Vue.extend({
    mixins: [myMixin]
})

var component = new Component()
```