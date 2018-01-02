---
title: Vue.js脚手架快速搭建项目
categories: 前端学习笔记
tags: VueJS
comment: true
date: 2017-06-28 20:26:07
---
vue.js的安装及使用脚手架工具快速搭建项目结构。

<!-- more -->

# 安装

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

# 命令行工具

```bash
# 全局安装 vue-cli
$ cnpm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 这里需要进行一些配置，默认回车即可
......
$ cd my-project
$ npm install
$ npm run dev
```

# 参考资料：

- [Webpack 入门教程](http://www.runoob.com/w3cnote/webpack-tutorial.html)
- [官方文档](http://vuejs.org/v2/guide/syntax.html)
- [中文文档](https://cn.vuejs.org/v2/guide/syntax.html)