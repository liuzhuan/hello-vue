# Vue-Cli 工具

vue-cli 是一个简单的脚手架命令行工具，用于快速创建 vue 系项目。

## 安装

安装前需要确保 Node.js 的版本号 >= 6.x，8.x 为佳。另外需要 npm 3+，以及 git 。

```bash
npm install -g vue-cli
```

## 使用方法

```bash
vue init <template-name> <project-name>
```

比如：

```bash
vue init webpack my-project
```

上面命令从 [vuejs-templates/webpack][webpack] 拉取模板，提示一些信息，然后在 `./my-project/` 生成项目结构。

## REF

- [vue-cli @ github][cli]
- [vuejs-templates][webpack]

[cli]: https://github.com/vuejs/vue-cli
[webpack]: https://github.com/vuejs-templates/webpack