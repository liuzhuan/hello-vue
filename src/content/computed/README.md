# 计算属性和观察者

## 计算属性

模板内嵌表达式虽然方便，但仅适合简单运算。逻辑复杂表达式不易维护，比如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

对于复杂逻辑，弊端有二：不易弄懂其逻辑；不便多次复用。

对于任何复杂逻辑，你应该使用计算属性（`computed property`）。

**简单例子**

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    reversedMessage: function() {
      return this.message.split('').reverse().join('')
    }
  }
})
```

**计算属性 vs 方法**

从最终结果上看，计算属性和方法完全相同。唯一区别在于，计算属性是缓存的。只要它依赖的属性不变，多次调用计算属性，不会重新计算。

这意味着如下例子中的计算属性不会更新，因为 `Date.now()` 不是响应式依赖：

```js
computed: {
  now: function() {
    return Date.now()
  }
}
```

## **计算属性 vs 被观察属性**

Vue 确实提供了一个更通用的方法来观察 Vue 实例的数值，并对其变化作相应：观察属性（`watch properties`）。观察属性容易被滥用，尤其 Angular 人士。然而，通常使用计算属性更佳。比如：

```html
<div id="demo">{{ fullName }}</div>
```

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  }, 

  watch: {
    firstName: function(val) {
      this.fullName = val + ' ' + this.lastName
    },

    lastName: function(val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

以上代码是命令式且重复，与计算属性版本做比较：

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function() {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

**计算 Setter**

计算模型默认只读，若要需要，可以设置其可写：

```js
computed: {
  fullName: {
    get: function() {
      return this.fullName + ' ' + this.lastName
    },

    set: function(newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

现在，当你执行 `vm.fullName = 'John Doe'` 时，会唤起 setter ，并且会自动更新 `vm.firstName` 和 `vm.lastName` 。

## 观察者

计算属性可满足大多数场景，但有时自定义的观察者函数也是必须的。这也是 Vue 提供通用响应选项 `watch` 的原因。一般适合处理异步回调或计算量复杂的操作。

比如：

```html
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>

<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

```js
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    question: function(newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },

  methods: {
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mask. ;-)'
          return
        }

        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function(response){
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function(error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      500
    )
  }
})
```

除了使用 `watch` 选项，也可以使用命令式的 `vm.$watch` API。

## REF

- [Computed Properties and Watchers][guide]

[guide]: https://vuejs.org/v2/guide/computed.html