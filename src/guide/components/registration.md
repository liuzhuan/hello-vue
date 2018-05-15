# 组件注册

## 组件名称

组册组件必须要有组件名称。比如，在全局注册中：

```js
Vue.component('my-component-name', { /* ... */ })
```

组件名称是 `Vue.component` 的第一个参数。

组件名称推荐遵循 [W3C rules][w3c]，即全小写，中间可以包含短横线。具体可以参见[风格指南][style-guide]。

## 全局注册

TODO

## 局部注册

TODO

## 模块系统

TODO

### 模块系统的局部注册

TODO

### 基组件的自动全局注册

TODO

## REF

- [Component Registration][guide]
- [Style Guide][style-guide]

[guide]: https://vuejs.org/v2/guide/components-registration.html
[w3c]: https://www.w3.org/TR/custom-elements/#concepts
[style-guide]: https://vuejs.org/v2/style-guide/