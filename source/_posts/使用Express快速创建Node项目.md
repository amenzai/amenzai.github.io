---
title: 使用Express快速创建Node项目
categories: node
tags: node
comment: true
date: 2017-07-02 20:26:07
---

NodeJS的Express框架安装介绍。

<!-- more -->

# 安装

```bash
$ npm install -g express-generator #需先安装express-generator  
  
$ npm install -g express  
  
$ express -V #验证是否安装成功  
```

# 创建项目

```bash
$ express myfirstexpress #express的默认模版采用jade，若需要ejs模版支持，加上-e选项，即 express -e myfirstexpress  
$ cd myfirstexpress  
  
$ ls  
app.js  bin  package.json  public  routes  views #项目的目录结构   
```

# 运行项目

```bash
$ npm install #需要等待一段时间，因为需要获取很多的库文件  
  
$ npm start  
  
> myfirstexpress@0.0.1 start /root/myfirstexpress  
> node ./bin/www  
```

# 访问

浏览器中输入 http://你的IP:3000