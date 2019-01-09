# 使用 Vue CLI 3

[online reading](https://cli.vuejs.org/)

包名从 `vue-cli` 改为 `@vue/cli`，如果曾经安装过 `vue-cli`（1.x 或 2.x），需要先卸载：

```sh
$ npm uninstall vue-cli -g
# OR
$ yarn global remove vue-cli
```

Vue CLI 依赖 Node.js v8.9+，推荐使用 v8.11.0+

```sh
$ npm install -g @vue/cli
# OR
$ yarn global add @vue/cli

# Create a project
vue create my-project
# OR
vue ui
```

如果要升级全局安装的 `@vue/cli` 版本

```sh
$ vue --version
3.2.1

$ npm update -g @vue/cli
# after a loooooong time update

$ vue --version
3.3.0
```

## 系统组件

Vue CLI 包含如下组件

| 组件        | 包名                                       | 备注                                         |
| ----------- | ------------------------------------------ | -------------------------------------------- |
| CLI         | `@vue/cli`                                 | 提供 `vue` 命令                              |
| CLI Service | `@vue/cli-service`                         | 基于 webpack 和 webpack-dev-server，提供服务 |
| CLI Plugins | `@vue/cli-plugin-*` 或 `@vue-cli-plugin-*` | 官方或社区开发的插件                         |

