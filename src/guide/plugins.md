# 插件

[online reading](https://vuejs.org/v2/guide/plugins.html)

## Code Demo

Using a Plugin

```js
Vue.use(MyPlugin);

new Vue({})
```

Writing a Plugin

```js
MyPlugin.install = function(Vue, options) {
    Vue.myGlobalMethod = function() {
        // some logic
    }
    
    Vue.directive('my-directive', {
        bind(el, binding, vnode, oldVnode) {
	    // do something
	}
    })
    
    Vue.mixin({
        created: function() {
	    // some logic...
	}
    })
    
    Vue.prototype.$myMethod = function(methodOptions) {
        // some logic...
    }
}
```