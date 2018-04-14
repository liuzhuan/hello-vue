# MVVM 原理

Vue 的数据绑定主要通过**数据劫持+发布订阅模式**实现，使用 ES5 的 `Object.defineProperty`。

使用方法如下：

```js
let language = { name: 'C' }

Object.defineProperty(language, 'birthday', {
  value: '1972',
  enumerable: true,
  writable: true,
  configurable: true
})

Object.defineProperty(language, 'author', {
  enumerable: true,
  configurable: true,
  get () {
    return 'Dennis M. Ritchie'
  },
  set (val) {
    this._author = val
  }
})
```

注意，`get`、`set` 称作存取器函数，它们不可以和 `value`、`writable` 同时存在，否则会报错。

具体的 MVVM 代码可以参见 [index.html](./index.html) 和 [mvvm.js](./mvvm.js)。

## REF

- [不好意思！耽误你的十分钟，让MVVM原理还给你 - 掘金][chenhongdong]，chenhongdong，2018/04/01

[chenhongdong]: https://juejin.im/post/5abdd6f6f265da23793c4458