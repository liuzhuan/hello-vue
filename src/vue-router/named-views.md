# 命名视图

有时候需要同时展示多个视图，比如，创建了一个 `sidebar` 视图和一个 `main` 视图。此时，我们不需要一个出口，而是需要多个出口，每个出口有个名称。没有名称的 `router-view` 的名称是 `default`。

```html
<router-view class="view one"></router-view>
<router-view class="view two"></router-view>
<router-view class="view three"></router-view>
```

每个视图需要使用一个组件渲染，因此对于相同路由，多个视图需要多个组件。务必使用 `components`（注意复数后缀 `s`） 选项：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar, 
        b: Baz
      }
    }
  ]
})
```

## 嵌套命名视图

结合命名视图和嵌套视图可以创建复杂的版式。当这么做时，你需要为嵌套的 `router-view` 命名。我们来看一个设置面板的例子：

```
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings                      |                  | UserSettings                 |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>  | | Nav | UserProfile        | |
| |     +-------------------------+ |                  | |     +--------------------+ |
| |     |                         | |                  | |     | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

- `Nav` 只是一个常规组件
- `UserSettings` 是一个视图组件
- `UserEmailsSubscriptions`, `UserProfile`, `UserProfilePreview` 是嵌套视图组件。

上图板式的 `UserSettings` 组件的 `<template>` 可以用如下代码编写：

```html
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

使用如下路由设置就可以达到该版式：

```js
{
  path: '/settings',
  component: UserSettings,
  children: [
    {
      path: 'emails',
      component: UserEmailsSubscriptions
    },
    {
      path: 'profile',
      components: {
        default: UserProfile,
        helper: UserProfilePreview
      }
    }
  ]
}
```

## REF

- [Named Views][named] - vuejs

[named]: https://router.vuejs.org/en/essentials/named-views.html