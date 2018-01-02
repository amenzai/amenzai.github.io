---
title: 如何写一个属于自己的gitbook
categories: 前端学习笔记
tags: gitbook
comment: true
date: 2017-11-11 20:26:07
---
来吧！教你用gitbook写本属于自己的书-。-

<!-- more -->

**第一步**
```bash
# 安装淘宝镜像，下载相关包快一些
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm config set registry https://registry.npm.taobao.org/
```

**第二步**
```bash
# 全局安装gitbook
$ cnpm install gitbook -g
```

**第三步**
```bash
# 创建一个目录，进入
$ mkdir gitbook-demo
$ cd gitbook-demo
# 初始化书籍目录
$ gitbook init 
# 编译书籍
$ gitbook serve 
```

**说明一下：**

init 以后，目录里会有这两个文件`README.md` 和 `SUMMARY.md` ，README.md 是对书籍的简单介绍，SUMMARY.md 是书籍的目录结构。

目录结构长这样：

```md
* [javascript简介](README.md)
* [语法](chapter1/README.md)
    * [基本语法](chapter1/section1.1.md)
    * [数据类型](chapter1/section1.2.md)
    * [数值](chapter1/section1.3.md)
    * [字符串](chapter1/section1.4.md)
    * [对象](chapter1/section1.5.md)
    * [数组](chapter1/section1.6.md)
    * [函数](chapter1/section1.7.md)
    * [运算符](chapter1/section1.8.md)
    * [数据类型转换](chapter1/section1.9.md)
    * [错误处理机制](chapter1/section1.10.md)
    * [编程风格](chapter1/section1.11.md)
```

编写SUMMARY.md，然后 `gitbook init`生成目录结构文件，然后编写各个文件夹中生成的文件。

最后`gitbook serve`。

gitbook serve 命令实际上会首先调用 `gitbook build` 编译书籍，完成以后会打开一个 web 服务器，监听在本地的 `4000` 端口。

**小拓展**

你可以书籍提交github，在托管书籍的仓库建一个`gh-pages`分支，将本地编译好的书籍文件（就是那个`_book`目录里的文件）上传到这个分支，然后就可以使用这个网址访问

> http://yourUserName.github.io/仓库名

当然也可以发布到[gitbook](https://www.gitbook.com/)，然而这个网站访问有点慢-.-

