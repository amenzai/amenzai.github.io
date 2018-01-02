---
title: AMD规范
categories: 前端学习笔记
tags: [AMD规范,模块化编程]
comment: true
date: 2017-04-03 20:26:07
---
这一篇讲解一下AMD规范。

<!-- more -->

目前，通行的Javascript模块规范共有两种：CommonJS和AMD。

有了模块，我们就可以更方便地使用别人的代码，想要什么功能，就加载什么模块。但是大家必须以同样的方式编写模块。

# CommonJS
node.js的模块系统，就是参照CommonJS规范实现的。在CommonJS中，有一个全局性方法require()，用于加载模块。

加载模块，调用模块的方法示例：
```javascript   
var math = require('math');
　　math.add(2,3); // 5
```

# AMD
AMD也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：
```javascript   
require([module], callback);

require(['math'], function (math) {
　　　　math.add(2, 3);
　　});
```