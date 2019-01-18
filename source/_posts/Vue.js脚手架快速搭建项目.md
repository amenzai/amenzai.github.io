---
title: Vue安装及使用脚手架快速搭建项目
categories: 前端学习笔记
tags: VueJS
comment: true
date: 2017-06-28 20:26:07
---
vue.js的安装及使用脚手架工具快速搭建项目结构。

<!-- more -->

## 安装

- [官网下载](http://vuejs.org/js/vue.min.js)
- CDN
    - [BootCDN](https://cdn.bootcss.com/vue/2.2.2/vue.min.js)
    - [unpkg](https://unpkg.com/vue/dist/vue.js)
    - [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js)
- NPM：在用 Vue.js 构建大型应用时推荐使用 NPM 安装
```bash
# 最新稳定版
$ npm install vue
```

## 命令行工具

```bash
# 全局安装 vue-cli
$ cnpm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 这里需要进行一些配置，默认回车即可

# 启动
$ cd my-project
$ npm install
$ npm run dev
```

## 更新（vue-cli3）

**脚手架安装和构建**

```bash
# 安装 Vue CLI 3.x
npm i -g @vue/cli or yarn global add @vue/cli

# my-project 是你的项目名称
vue create my-project

# 打开项目目录
cd vue-project

# 启动项目 localhost:8080
yarn serve

# or
npm run serve

```

**目录结构**

```bash
├── node_modules     # 项目依赖包目录
├── public
│   ├── favicon.ico  # ico图标
│   └── index.html   # 首页模板
├── src 
│   ├── assets       # 样式图片目录
│   ├── components   # 组件目录
│   ├── views        # 页面目录
│   ├── App.vue      # 父组件
│   ├── main.js      # 入口文件
│   ├── router.js    # 路由配置文件
│   └── store.js     # vuex状态管理文件
├── .gitignore       # git忽略文件
├── .postcssrc.js    # postcss配置文件
├── babel.config.js  # babel配置文件
├── package.json     # 包管理文件
└── yarn.lock        # yarn依赖信息文件
```

**可视化界面**

```bash
vue ui # 自动打开本地 8000 端口
```

**vue-cli 包安装**

在上述的教程中，我们使用 npm 或 yarn 进行了包的安装和配置，除了以上两种方法，vue-cli 3.x 还提供了其专属的 vue add 命令，但是需要注意的是该命令安装的包是以 @vue/cli-plugin 或者 vue-cli-plugin 开头，即**只能安装 Vue 集成的包。**

比如运行：
```bash
vue add jquery
```
其会安装 vue-cli-plugin-jquery，很显然这个插件不存在便会安装失败。又或者你运行：

```bash
vue add @vue/eslint
```
其会解析为完整的包名 @vue/cli-plugin-eslint，因为该包存在所以会安装成功。

同时，不同于 npm 或 yarn 的安装， vue add 不仅会将包安装到你的项目中，其还会改变项目的代码或文件结构，所以安装前最好提交你的代码至仓库。

另外 vue add 中还有两个特例，如下：

```bash
# 安装 vue-router
vue add router

# 安装 vuex
vue add vuex
```
这两个命令会直接安装 vue-router 和 vuex 并改变你的代码结构，使你的项目集成这两个配置，并不会去安装添加 vue-cli-plugin 或 @vue/cli-plugin 前缀的包。

## 参考资料：

- [Webpack 入门教程](http://www.runoob.com/w3cnote/webpack-tutorial.html)
- [官方文档](http://vuejs.org/v2/guide/syntax.html)
- [中文文档](https://cn.vuejs.org/v2/guide/syntax.html)