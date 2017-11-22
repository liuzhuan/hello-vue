# Vue Cheat Sheet

> Vue is a progressive framework for building user interfaces.

## syntax

declarative rendering

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

app.message = 'foo bar' // everything is reactive
```

bind element attributes

```html
<div id='app'>
  <span v-bind:title="message">
    Hover your mouse over me to see my dynamically bound title!
  </span>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
```

## vue-cli

## REF

- https://vuejs.org/v2/guide/