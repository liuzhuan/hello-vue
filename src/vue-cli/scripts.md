# scripts 脚本

```json
{
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "e2e": "node test/e2e/runner.js",
    "test": "npm run unit && npm run e2e",
    "lint": "eslint --ext .js,.vue src test/unit test/e2e/specs",
    "build": "node build/build.js"
  }
}
```

## `dev`

`dev` 脚本使用了 `webpack-dev-server`，具体命令是：

```sh
webpack-dev-server --inline --progress --config build/webpack.dev.conf.js
```

[`--inline`][inline] 表示将热更新代码“内联”到你的最终打包文件中。这是推荐的用法，也是默认设置值。另一种模式是 iframe 模式，需要设置为 `--inline=false` 。

[`--progress`][progress] 表示在终端中显示运行进度。

`--config` 表示开发服务器使用的配置文件位于 `build/webpack.dev.conf.js` 。

### `build/webpack.dev.conf.js` 配置文件

（未完待续）

## REF

- [webpack/webpack-dev-server][dev-server]
- [DevServer Configuration][dev-server-config]

[dev-server]: https://github.com/webpack/webpack-dev-server
[dev-server-config]: https://webpack.js.org/configuration/dev-server/#devserver
[inline]: https://webpack.js.org/configuration/dev-server/#devserver-inline
[progress]: https://webpack.js.org/configuration/dev-server/#devserver-progress-cli-only