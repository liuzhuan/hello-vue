# Vue CLI 工具

> 本文主要介绍 Vue CLI 3.x

Vue CLI 是一个简单的脚手架命令行工具，用于快速创建 vue 项目。

Vue CLI 包含如下组件：

- CLI(`@vue/cli`) 全局安装的 npm 包，提供 `vue` 指令。子命令包含：
    - `vue create` 快速创建新目录
    - `vue serve` 快速试验原型
    - `vue ui` 图形界面管理面板
- CLI Service(`@vue/cli-service`) 基于 webpack 和 webpack-dev-server，类似 `create-react-app`。包含如下部分
    - 加载其他 CLI 插件的核心服务
    - 内部的 webpack 配置，已对大多数 app 进行了优化
    - `vue-cli-service` 二进制，提供 `serve`, `build` 和 `inspect` 命令
- CLI Plugins(`@vue/cli-plugin-*` 或 `vue-cli-plugin-*`) 提供额外功能的插件。

## 安装

如果已经安装 vue-cli，需要卸载：

```sh
$ npm uninstall vue-cli -g
# OR
$ yarn global remove vue-cli 
```

安装前需要确保 Node.js 的版本号 >= 8.9+，8.11.0+ 更好。

```sh
$ npm install -g @vue/cli
# OR
$ yarn global add @vue/cli
```

## 创建项目

```sh
$ vue create hello-world

# 在当前目录内创建
$ vue create .

# 使用图形界面
$ vue ui
```

## 插件和预设

Vue CLI 基于插件架构。插件可以修改内部 webpack 配置参数，也能将命令注入到 `vue-cli-service`。

每个 CLI 插件搭载一个生成器（用于创建文件）和一个运行时插件（用于调整核心 webpack 配置和注入命令）。

如果要在现有项目安装插件，可以执行 `vue add`：

```sh
$ vue add @vue/eslint

# add vue-router
$ vue add router

# add vuex
$ vue add vuex

# invoke plugin's generator
$ vue invoke vuex
```

Vue CLI 预设是一个 JSON 对象，包含预置的用于创建项目的选项和插件。

## CLI Service

[`vue-cli-service serve`](https://cli.vuejs.org/guide/cli-service.html#vue-cli-service-serve) 可以启动开发服务器（基于 webpack-dev-server），自带 HMR。

可以通过 `vue.config.js` 的 `devServer` 字段修改开发服务器参数。

[`vue-cli-service build`](https://cli.vuejs.org/guide/cli-service.html#vue-cli-service-build) 在 `dist/` 目录创建生产版本。

查看所有的命令：

```sh
$ npx vue-cli-service help
```

## 缓存和并行

对于 Vue/Babel/TypeScript 默认开启 `cache-loader`。文件缓存在 `node_modules/.cache`。如果遇到编译问题，可以尝试删除缓存目录。

如果开发机拥有多个 CPU，将对 Babel/TypeScript 开启 `thread-loader`。

## Git Hooks

`@vue/cli-service` 自动安装 `yorkie`，可以在 `package.json` 的 `gitHooks` 字段轻松配置 Git hooks

```json
{
    "gitHooks": {
        "pre-commit": "lint-staged"
    }
}
```

## 浏览器兼容性

通过 package.json 的 `browserslist` 字段（或 `.browserslistrc` 独立文件）指定项目面向的浏览器范围。

## HTML 和静态资源

`public/index.html` 是 `html-webpack-plugin` 需要的模板文件。可以在其中使用 lodash 模板风格的字符串：

- `<%= VALUE%>` 未转义的插值
- `<%- VALUE%>` HTML 转移的插值
- `<% expression %>` JavaScript 控制的语句

## [处理 CSS](https://cli.vuejs.org/guide/css.html)

TODO

## REF

- [Overview of Vue CLI](https://cli.vuejs.org/guide/)
- [Installation](https://cli.vuejs.org/guide/installation.html)
- [Plugins and Presets](https://cli.vuejs.org/guide/plugins-and-presets.html)
- [CLI Service](https://cli.vuejs.org/guide/cli-service.html)