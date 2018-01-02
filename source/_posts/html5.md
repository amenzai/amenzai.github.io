---
title: HTML5总结
categories: 前端学习笔记
tags: HTML5
comment: true
date: 2016-06-10 20:26:07
---
HTML5新增内容的整理。

<!-- more -->

# 新标签

## 更语义化的标签

```html
<header>
<nav>
<footer>
<main>
<aside>
<artical>
<section>
<figure>
<figcaption>
```
## 应用程序功能标签
```html
<!-- 模拟下拉列表 -->
<input list="teachers" />
<datalist id="teachers">
  <option value="段誉" />
  <option value="乔峰" />
  <option value="方世玉" />
</datalist>
<!-- 模拟菜单 仅仅火狐支持-->
<menu>
  <command type="command" disabled label="Publish" />
</menu>
<!-- 点击查看详情 -->
<details>
  <summary>HTML 5</summary>
  This slide deck teaches you everything you need to know about HTML 5.
</details>
<!-- meta -->
<meter min="0" max="100" low="40" high="90" optimum="100" value="91">A+</meter>
<!-- 进度条 在不同操作系统上显示不同 -->
<progress>working...</progress>
<progress value="75" max="100">3/4 complete</progress>
```

# 新属性
## ARIA 属性

```html
<!-- 盲人用户如何知道当前浏览区域就是网站主导航？ -->
<div id="mainnav" role="navigation">
  <a href="http://news.qq.com/" target="_blank">新闻</a>
  <a href="http://v.qq.com/" target="_blank">视频</a>
  <a href="http://ent.qq.com/" target="_blank">娱乐</a>
</div>
<!-- 如何让盲人用户知道我们使用li标签是用来模拟标准select控件？ -->
<div class="dropdown">
  <a href="javascript:;" role="combobox" aria-autocomplete="list" aria-owns="question-list" aria-expanded="true">选择提示问题</a>
  <ul id="question-list" role="listbox">
    <li role="option"><a href="javascript:;">我妈妈的名字是？</a></li>
    <li role="option"><a href="javascript:;">我爸爸的名字是？</a></li>
    <li role="option"><a href="javascript:;">我爱人的名字是？</a></li>
  </ul>
</div>
```
## data属性

```html
<!-- 标签绑定数据 -->
<li class="item" data-id="1" data-age="18" data-gender="true">text</li>
<script>
  // 设置data属性
  DOM.setAttribute('data-name', 'text');
  // 获取data属性
  DOM.dataset['age']
</script>
```

# 新的表单类型
```
type = ['search','date','range','number','color','email','url','tel']
```

# 多媒体元素
```html
h5-vedio or h5-audio //在sublime中定义好了代码段，直接输入+tab
<!-- 滚动字体 -->
<marquee style="width: 388px; height: 200px" scrollamount="2" direction="up">
  <p><span>日不落的夏天中了50元酒店信用住超值红包</span></p>
  <p><span>悠悠youyou中了20元酒店信用住超值红包</span></p>
  <p><span>xiaomogu中了100元酒店信用住超值红包</span></p>
</marquee>
```

# SVG
```html
//svg文件的引入方式
<!-- 第一种方式 -->
<iframe src="famoustiger.svg" style="border:none; width:550px; height:540px; background: white;"></iframe>
<!-- 第二种方式 -->
<embed src="famoustiger.svg" style="border:none; width:550px; height:540px; background: white;" type="image/svg+xml" pluginspage="http://www.adobe.com/svg/viewer/install/" />
<!-- 第三种方式 -->
<object type="image/svg+xml" style="border:none; width:550px; height:540px; background: white;" data="famoustiger.svg" codebase="http://www.adobe.com/svg/viewer/install/"></object>
```
SVG可以看成个页面，因此用上述方法引入时无法更改其样式。

所以通常我们会这样引入：通过Js将SVG文档根元素，插入到本文档，这样我们可以对其样式进行更改。
```html
<svg data-src="demo.svg"></svg>
var svgs = $('svg');
for (var i = 0; i < svgs.length; i++) {
  var src = $(svgs[i]).data('src');
  // 向服务器发送请求 得到svg
  $.get(src, function(data) {

    var el = data.documentElement;
    $(document.body).append($(el));
  });
}
```
# JS-API
## 新的选择器

```javascript 
document.querySelector(selector) //可以通过CSS选择器的语法找到匹配的第一个DOM元素

document.querySelectorAll(selector) //可以通过CSS选择器的语法找到匹配的所有DOM元素
```
## Class List

```javascript 
var demoClassList = demoElement.classList; //返回一个数组
demoClassList.add('bordered'); // 添加一个类名
demoClassList.remove('highlighted'); // 删除一个类名
var isAnimated = demoClassList.contains('animated'); // 获取是否存在指定类名
demoClassList.toggle('animated', !isAnimated); // 根据第二个参数切换一个类名
```
## history-api

界面上的所有JS操作不会被浏览器记住，就无法回到之前的状态。

在HTML5中可以通过window.history操作访问历史状态，让一个页面可以有多个历史状态
```javascript 
window.history.forward(); // 前进
window.history.back(); // 后退
window.history.go(); // 刷新
history.pushState(放入历史中的状态数据, 设置title(现在浏览器不支持)， 改变历史状态)
window的popstate事件当前进或后退触发。
e.state得到“放入历史中的状态数据”
单页应用程序（记录js中的事件，可以返回）
```
## 全屏 API
JavaScript中可以通过调用`requestFullScreen()`方式实现指定元素的全屏显示。
```javascript 
var element = document.querySelector('...');
element.requestFullScreen();

fullScreen(element)
// 让元素全屏的方法，全平时背景是黑的，要给每个元素设置背景才行
function fullScreen(element) {
  if (element.webkitRequestFullScreen) {
    element.webkitRequestFullScreen();
  } else if (element.mozRequestFullScreen) {
    element.mozRequestFullScreen();
  } else if (element.requestFullScreen) {
    element.requestFullScreen();
  }
  // 锁定鼠标
  if (element.requestPointerLock)
    element.requestPointerLock();
}
```
## Application Cache

对HTML元素加一个manifest属性，根目录创建一个cache.manifest文件，高速浏览器将会缓存里面编写的文件，以后不用到服务器加载，版本更新的话，把里面的version改了就行。
```html
<html lang="zh-CN" manifest="cache.manifest">
<!-- cache.manifest文件内容 -->
CACHE MANIFEST
# version 1.0.7

CACHE:
  cache.css //路径写正确
  cache.js
  05-application-cache.html
  toy.png

NETWORK:
  *
```
## Web Storage(本地存储)

```javascript 
localStorage //永久存储，除非用户删除了
sessionStorage //网页关闭即删除
localStorage.setItem(key,value); //设置键值对
localStorage.getItem(key); 获取存储的键值
```
