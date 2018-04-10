# 模板语法

Vue.js 模板语法基于 HTML，可以声明式的将 DOM 绑定到 Vue 实例的数据。

Vue 会把模板编译为虚拟 DOM 渲染函数。结合响应式系统，Vue 可以智能推断，尽量减小需要重新渲染的组件数量。

如果你精通虚拟 DOM，习惯原生 JavaScript，也可以直接[编写渲染函数][render]，甚至使用 JSX 语法。

## 插值

**字符串文本**

使用双大括号：

```html
<span>Message: {{ msg }}</span>
```

还可以使用 `v-once` 指令，让数据只绑定一次，后续更新不会触发渲染：

```html
<span v-once>This will never change: {{ msg }}</span>
```

**HTML 插值**

使用 `v-html` 指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

`span` 内容会被 `rawHtml` 属性替换，解释为普通 HTML - 其中的数据绑定会被忽略。不要将 `v-html` 当作模板片段，因为 Vue 不是字符串模板引擎。而是要把组件当作 UI 复用的单元。

⚠️ 注意：在网站上动态渲染任意 HTML 极度危险，容易造成跨站脚本攻击（XSS）。仅使用可靠内容作为 HTML 插值，**绝对不要对用户提供的内容使用 HTML 插值**。

**属性 Attributes**

双括号不可以在 HMTL 属性中使用，需要替换为 `v-bind` 指令：

```html
<div v-bind:id="dynamicId"></div>
```

布尔值稍有不同，只要值存在，就是 `true`。比如：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 值是 `null`, `undefined` 或 `false`，`disabled` 属性甚至都不会出现在 `<button>` 中。

**JavaScript 表达式**

Vue.js 的数据绑定支持所有的 JavaScript 表达式功能：

```html
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```

⚠️ 注意：模板表达式在沙箱中运行，只能访问白名单全局变量，比如 `Math` 和 `Date` 等。不可以访问用户定义的全局变量。

## 指令

指令是以 `v-` 开头的特殊属性，其值应该是一条 JS 表达式（`v-for` 例外）。指令的指责是对 DOM 施加作用。比如：

```html
<p v-if="seen">Now you see me</p>
```

**参数**

有些指令可以有参数，在指令名之后添加冒号即可。比如，`v-bind` 指令用来响应式的刷新一个 HTML 属性：

```html
<a v-bind:href="url">...</a>
```

另一个例子是 `v-on` 指令，用来监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

**修饰器**

修饰器是一种特殊后缀，使用 `.` 标示。用来指示指令应该绑定到一些特殊方式。比如，`.prevent` 修饰器告诉 `v-on` 指令在事件中执行 `event.preventDefault()`。

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

## 缩写形式

`v-` 前缀用作标示 Vue 相关属性的视觉提示。Vue.js 提供了 `v-bind` 和 `v-on` 两者的缩写形式：

```html
<!-- v-bind 语法 -->
<a v-bind:href="url"> ... </a>
<a :href="url"> ... </a>

<!-- v-on 语法 -->
<a v-on:click="doSomething"> ... </a>
<a @click="doSomething"> ... </a>
```

## REF

- [Template Syntax][tpl]
- [Render Functions & JSX][render]

[tpl]: https://vuejs.org/v2/guide/syntax.html
[render]: https://vuejs.org/v2/guide/render-function.html