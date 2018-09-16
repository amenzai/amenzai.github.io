---
title: 发布自己的npm包
categories: 前端学习笔记
tags: [node, npm]
comment: true
date: 2017-12-01 20:26:07
---

封装自己常用的工具方法，做成一个npm包，发布到npm官网，这样下次用就可以直接`npm install packageName`使用了。

如何编写一个包，不详细说了，其实就是`npm init`创建一个`package.json`文件，写下相关信息，需要注意的是里面的`main`属性，它指明了入口文件，我们在`require`包的时候，其实就是`require`这个文件。

相信了解commonjs规范，做一个包并不难。

---

重点讲发布

<!-- more -->

## npm adduser
在npmjs.com注册一个用户。

```bash
$ npm adduser
Username: YOUR_USER_NAME
Password: YOUR_PASSWORD
Email: YOUR_EMAIL@domain.com
```

## npm publish
将当前模块发布到`npmjs.com`。·

```bash
# 先登录
$ npm login

# 发布
$ npm publish

# 给发布的包加标签 例如：beta
$ npm publish --tag beta
```

默认的发布标签是latest。

如果模块用ES6写的，发布的时候最好转为ES5。

```bash
$ npm install --save-dev babel-cli@6 babel-preset-es2015@6

# package.json
"scripts": {
  "build": "babel source --presets babel-preset-es2015 --out-dir distribution",
  "prepublish": "npm run build"
}
```
运行上面的脚本，会将source目录里面的ES6源码文件，转为distribution目录里面的ES5源码文件。然后，在项目根目录下面创建两个文件.npmignore和.gitignore，分别写入以下内容。

```js
// .npmignore
source
// .gitignore
node_modules
distribution
```

## npm owner

模块的维护者可以发布新版本。npm owner命令用于管理模块的维护者。

```bash
# 列出指定模块的维护者
$ npm owner ls <package name>
# 新增维护者
$ npm owner add <user> <package name>
# 删除维护者
$ npm owner rm <user> <package name>
```

**注意事项：**

npm 仓库代理要设置成这个
```bash
npm set registry http://registry.npmjs.org
```

修改包以后，更改package.json里的version就行了