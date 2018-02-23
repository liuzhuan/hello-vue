# 目录结构

```
.
├── build/                      # webpack 配置文件
│   └── ...
├── config/
│   ├── index.js                # 主项目配置
│   └── ...
├── src/
│   ├── main.js                 # app 入口文件
│   ├── App.vue                 # 主 app 组件
│   ├── components/             # ui 组件
│   │   └── ...
│   └── assets/                 # 模块资源（经 webpack 处理）
│       └── ...
├── static/                     # 纯静态资源（直接拷贝）
├── test/
│   └── unit/                   # 单元测试
│   │   ├── specs/              # 测试 spec 文件
│   │   ├── eslintrc            # eslint 配置文件，单元测试部分
│   │   ├── index.js            # 测试构建入口文件
│   │   ├── jest.conf.js        # jest 测试配置文件
│   │   └── karma.conf.js       # Karma 单元测试配置文件
│   │   ├── setup.js            # Jest 测试准备文件
│   └── e2e/                    # e2e 测试
│   │   ├── specs/              # 测试 spec 文件
│   │   ├── custom-assertions/  # e2e 测试的自定义断言
│   │   ├── runner.js           # 测试运行器脚本
│   │   └── nightwatch.conf.js  # 测试运行器配置文件
├── .babelrc                    # babel 配置
├── .editorconfig               # 缩进，空格/制表及其他类似编辑器设置
├── .eslintrc.js                # eslint 配置
├── .eslintignore               # eslint 忽略规则
├── .gitignore                  # 实用的默认 gitignore 文件
├── .postcssrc.js               # postcss 设置
├── index.html                  # index.html 模版
├── package.json                # 构建脚本和依赖
└── README.md                   # 默认的 README 文件
```

## REF

- [Project Structure][structure]

[structure]: https://vuejs-templates.github.io/webpack/structure.html