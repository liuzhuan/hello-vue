# HTML5 历史模式

`vue-router` 的默认是*哈希模式（`hash mode`）*，它使用 URL 哈希值模拟一个完整 URL，这样当 URL 变化时页面不会刷新。

为了去掉哈希，我们可以使用路由器的**历史模式（`history mode`）**，它会利用 `history.pushState` API 接口达到无刷新页面的目的。

```js
const router = new VueRouter({
  mode: 'history', 
  routes: [...]
})
```

当使用历史模式时，URL 会看起来像“普通”页面，比如：`http://oursie.com/user/id`。漂亮！

但随之而来的问题是：由于我们的 app 是一个单页应用，如果没有合理的服务器配置，用户会得到 404 报错，如果他们直接访问 `http://oursite.com/user/id`。这不太漂亮。

不用担忧，为了解决这个问题，你只需在服务器端增加一条简单的匹配所有的后备路由。如果 URL 没有匹配任意静态资源，它将返回 app 的 `index.html`。再次漂亮！

## 服务器设置样例

### Apache

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

### nginx

```
location / {
  try_files $uri $uri/ /index.html
}
```

### 原生的 Node.js

```js
const http = require('http')
const fs = require('fs')
const httpPort = 80

http.createServer((req, res) => {
  fs.readFile('index.htm', 'utf-8', (err, content) => {
    if (err) {
      console.log('We cannot open "index.htm" file.')
    }

    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' })

    res.end(content)
  })
}).listen(httpPort, () => {
  console.log('Server listening on: http://localhost:%s', httpPort)
})
```

### Express

对于 Node.js/Express，可以考虑使用 [connect-history-api-fallback middle][express-middleware]。

## 警告

需要提醒一下，你的服务器将不会出现 404 报警。为了解决这个问题，你需要在 Vue app 中实现一个全方位的路由，来显示 404 页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```

## REF

- [HTML5 History Mode][history-mode]

[history-mode]: https://router.vuejs.org/en/essentials/history-mode.html
[express-middleware]: https://github.com/bripkens/connect-history-api-fallback