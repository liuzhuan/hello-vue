# 渲染函数和 JSX

[online reading](https://vuejs.org/v2/guide/render-function.html)

1. 在渲染函数 `render` 中，`createElement` 并不会创建实际 DOM 节点，只是创建节点的描述信息。
2. TODO

## Code Demo

```js
Vue.component('anchored-heading', {
    render: function(createElement) {
        return createElement(
	    'h' + this.level,
	    this.$slots.default
	)
    },
    props: {
        level: {
	    type: Number,
	    required: true,
	}
    }
})
```

createElement 的签名如下：

```js
// @return VNode
createElement(
    // {String | Object | Function}
    // HTML 标签名称，组件选项或者返回这些数据的异步函数
    'div',
    // 数据选项对象
    {},
    [
        'Some text comes first',
	createElement('h1', 'A headline'),
	createElement(MyComponent, {
	    props: {
	        someProp: 'foobar'
	    }
	})
    ]
)
```