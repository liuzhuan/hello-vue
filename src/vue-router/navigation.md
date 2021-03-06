# 程序跳转

除了使用 `<router-link>` 声明式创建链接导航，还可以使用程序控制路由的跳转。

## `router.push(location, onComplete?, onAbort?)`

使用 `router.push` 可以跳转到另一个路由。在浏览器历史记录增加一个条目，因此点击浏览器回退按钮，可以返回上一页。

这也是点击 `<router-link>` 内部调用的方法。因此以下两种方式等价：

| 声明式                    | 函数式             |
| ------------------------- | ------------------ |
| `<router-link :to="...">` | `router.push(...)` |

参数可以是字符串，也可以是路径描述对象。比如：

```js
router.push('home')
router.push({ path: 'home' })
// 命名路由
router.push({ name: 'user', params: { userId: 123 } })
// 结果为 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' } })
```

注意，当提供了 `path` 后，`params` 会被忽略。`params` 和 `query` 是两个完全不同的参数。具体含义如下：

```js
const userId = 123
router.push({ name: 'user', params: { userId } })  // -> /user/123
router.push({ path: `/user/${userId}` })           // -> /user/123
// 如下例子不能如愿
router.push({ path: '/user', params: { userId } }) // -> /user
```

> 注意，如果目标路由和当前路由一致，仅参数不同，需使用 `beforeRouteUpdate` 监听路由变化。

2.2.0+ 提供了可选的 `onComplete` 和 `onAbort` 回调函数。

## `router.replace(location, onComplete?, onAbort?)`

和 `router.push` 类似，只是没有新增条目，只是替换当前条目而已。以下两者等价。

| 声明式                            | 函数式                |
| --------------------------------- | --------------------- |
| `<router-link :to="..." replace>` | `router.replace(...)` |

## `router.go(n)`

类似 `window.history.go(n)`，在历史记录中前进或后退多少步。

```js
router.go(1)
router.go(-1)
router.go(3)
```

## 操纵浏览历史

`router.push`, `router.replace` 和 `router.go` 和 `window.history` API 类似，但是可在各种路由模式下（`history`, `hash`, `abstract`）使用。

## REF

- [Programmatic Navigation][navigation]

[navigation]: https://router.vuejs.org/en/essentials/navigation.html