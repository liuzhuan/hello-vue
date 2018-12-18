# 使用插槽 slot

[online reading](https://vuejs.org/v2/guide/components-slots.html)

Vue 使用 `<slot>` 元素作为内容分发插槽。

```xml
<navigation-link url="/profile">
    Your Profile
</navigation-link>
```

其中，`<navigation-link>` 的模板如下：

```xml
<a
   :href="url"
   class="nav-link">
    <slot></slot>
</a>
```

当渲染组件时，`<slot>` 元素将被替换为 `Your Profile`。

如果 `<navigation-link>` 模板中不包含 `<slot>` 元素，其中的任何元素都将被丢弃。

## 命名插槽 Named Slots

有时需要多个插槽。比如，`<base-layout>` 组件的模板定义如下：

```xml
<div class="container">
    <header>
        <slot name="header"></slot>
    </header>
    <main>
        <slot></slot>
    </main>
    <footer>
        <slot name="footer"></slot>
    </footer>
</div>
```

可以在 `<template>` 上使用 `<slot>` 属性，指定命名插槽：

```xml
<base-layout>
    <template slot="header">
        <h1>Here might be a page title</h1>
    </template>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
    <template slot="footer">
        <p>Here's some contact info</p>
    </template>
</base-layout>
```

`slot` 也可以直接应用在普通元素：

```xml
<base-layout>
    <h1 slot="header">Here might be a page title</h1>
    
    <p>A paragragh for the main content.</p>
    <p>And another one.</p>
    
    <p slot="footer">Here's some contact info</p>
</base-layout>
```

## 默认插槽内容 Default Slot Content

```xml
<button type="submit">
    <slot>Submit</slot>
</button>
```

## 作用域插槽 Scoped Slots

有时候，需要在组件中提供插槽，能够获取子组件的数据。比如，`<todo-list>` 模板如下：

```xml
<ul>
    <li
        v-for="todo in todos"
        :key="todo.id">
        <slot :todo="todo">
            {{ todo.text }}
        </slot>
    </li>
</ul>
```

通过增加 `slot-scope` 可以在子组件获取数据：

```xml
<todo-list :todos="todos">
    <template slot-scope="slotProps">
        <span v-if="slotProps.todo.isComplete">:D</span>
        {{ slotProps.todo.text }}
    </template>
</todo-list>
```

在 `slot-scope` 中可以使用解构语法。