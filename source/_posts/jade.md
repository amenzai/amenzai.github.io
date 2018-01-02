---
title: jade使用介绍
categories: 前端学习笔记
tags: jade
comment: true
date: 2016-10-10 20:26:07
---
jade使用总结。

<!-- more -->

# jade基础语法知识

## 文档声明和头尾标签
jade编写：
```jade
doctype html
html
  head
    title my jade template
  body
    h1 Hello jade
```
编译后：
```html
<!DOCTYPE html>
<html>
  <head>
    <title>my jade template</title>
  </head>
  <body>
    <h1>Hello jade</h1>
  </body>
</html>
```
## 命令行适时编译
```c
jade index.jade  //编译后备压缩
jade index.jade -P  //编译文件为jade.html并且格式化了 放在相同的目录。
jade index.jade -P -w //实时编译
jade index.jade -P -w --obj '{"course","jade"}' //想文档中传递变量 优先级最低。
jade index.jade -P -O jade.json //通过json想文档传递数据
```

## 标签语法

jade编写：
```jade
h3 标签语法
section
  div
    ul
    p
```
编译后：
```html
<h3>标签语法</h3>
<section>
  <div>
    <ul></ul>
    <p></p>
  </div>
</section>
```
## 属性文本和值
jade编写：
```jade
h3 元素属性
#id.class1(class='class2')
div#id.class1.class2
  a(href='http://imooc.com', target='_blank') link

h3 元素的值，文本
p
  a(href='http://imooc.com',title='imooc jade study', data-uid='1000') link
  input(name='course', type='text', value='jade')
  input(name='type', type='checkbox', checked)
```
编译后：
```html
<h3>元素属性</h3>
  <div id="id" class="class1 class2"></div>
  <div id="id" class="class1 class2"><a href="http://imooc.com" target="_blank">link</a></div>
  <h3>元素的值，文本</h3>
  <p><a href="http://imooc.com" title="imooc jade study" data-uid="1000">link</a>
    <input name="course" type="text" value="jade">
    <input name="type" type="checkbox" checked>
  </p>
```
## 混合的成段文本和标签
jade编写：
```jade
h3 混排的大段文本
  p.
    1. aa
    2. bb
    <strong>333</strong>
    3. c
    4. dd
  p
    | 1. aa
    strong 11
    | 2. bb
    | 3. c
```
编译后：
```html
<h3>混排的大段文本</h3>
  <p>
    1. aa
    2. bb
    <strong>333</strong>
    3. c
    4. dd
  </p>
  <p>1. aa<strong>11</strong>2. bb
    3. c
  </p>
```
## 注释和条件注释
jade编写：
```jade
h2 注释
  h3 单行注释
  // h3.title(id='title', class='title3') imooc
  h3 非缓冲注释
  //- #id.classname
  h3 块注释
  //-
    p
      a(href='http://imooc.com',title='imooc jade study', data-uid='1000') link
      input(name='course', type='text', value='jade')
      input(name='type', type='checkbox', checked)

  <!--[if IE 8]>
  script(src='/ie.js')
  <![endif]-->    
```
编译后：
```html
<h2>注释</h2>
<h3>单行注释</h3>
<!-- h3.title(id='title', class='title3') imooc-->
<h3>非缓冲注释</h3>
<h3>块注释</h3>

<!--[if IE 8]>
<script src="/ie.js"></script><![endif]-->
```
## 变量声明和数据传递
jade编写：
```jade
h2 声明变量和替换数据-安全转义与非转义
  h3 转义
  - var data = 'text'
  - var htmlData = '<script>alert(1);</script><span>script</span>;'
  p #{data}
  p #{htmlData}
  p= data
  p= htmlData

  h3 非转义
  p!= htmlData
  p !{htmlData}

  h3 非解析变量符
  p \#{htmlData}
  p \!{htmlData}

  h3 不输出 undefined
  input(value='#{newData}')
  input(value=newData)

  h3 样式和脚本块语法
  style.
    body {color: #ff6600;}
  script.
    var imoocCourse = 'jade';
```
编译后：
```html
<h2>声明变量和替换数据</h2>
  <h3>转义</h3>
  <p>text</p>
  <p>&lt;script&gt;alert(1);&lt;/script&gt;&lt;span&gt;script&lt;/span&gt;;</p>
  <p>text</p>
  <p>&lt;script&gt;alert(1);&lt;/script&gt;&lt;span&gt;script&lt;/span&gt;;</p>
  <h3>非转义</h3>
  <p><script>alert(1);</script><span>script</span>;</p>
  <p><script>alert(1);</script><span>script</span>;</p>
  <h3>非解析变量符</h3>
  <p>#{htmlData}</p>
  <p>!{htmlData}</p>
  <h3>不输出 undefined</h3>
  <input value="undefined">
  <input>

  <h3>样式和脚本块语法</h3>
  <style>body {color: #ff6600;}</style>
  <script>
    var imoocCourse = 'jade';   
  </script>
```

## 流程逻辑
jade编写：
```jade
h2 流程逻辑
h3 if else
- var isImooc = true
- var lessons = ['jade', 'node']
if lessons
  if lessons.length > 2
    p more than 2: #{lessons.join(', ')}
  else if lessons.length > 1
    p more than 1: #{lessons.join(', ')}
  else
    p no lesson
else
  p no lesson

h3 unless
unless !isImooc
  p #{lessons.length}

h3 case
- var name = 'jade'
case name
  when 'java'
  when 'node'
    p Hi node!
  when 'jade': p Hi jade!
  when 'express': p Hi exress
  default
    p Hi #{name}

h3 for
- var imooc = {course: 'jade', level: 'high'}
- for (var k in imooc)
  p= imooc[k]

h3 each
- each value, key in imooc
  p #{key}: #{value}
each value, key in imooc
  p #{key}: #{value}

h3 遍历数组
- var courses = ['node', 'jade', 'express']
each item in courses
  p= item

h3 嵌套循环
- var sections = [{id: 1, items: ['a', 'b']}, {id: 2, items: ['c', 'd']}]
dl
  each section in sections
    dt= section.id
    each item in section.items
      dd= item

h3 while
- var n = 0
ul
  while n < 4
    li= n++
```
编译后：
```html
<h2>流程逻辑</h2>
  <h3>if else</h3>
  <p>more than 1: jade, node</p>
  <h3>unless</h3>
  <p>2</p>
  <h3>case</h3>
  <p>Hi jade!</p>
  <h3>for</h3>
  <p>jade</p>
  <p>high</p>
  <h3>each</h3>
  <p>course: jade</p>
  <p>level: high</p>
  <p>course: jade</p>
  <p>level: high</p>
  <h3>遍历数组</h3>
  <p>node</p>
  <p>jade</p>
  <p>express</p>
  <h3>嵌套循环</h3>
  <dl>
    <dt>1</dt>
    <dd>a</dd>
    <dd>b</dd>
    <dt>2</dt>
    <dd>c</dd>
    <dd>d</dd>
  </dl>
  <h3>while</h3>
  <ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
```

## 神奇的mixins
jade编写：
```jade
h2 神奇的 mixins
mixin lesson
  p imooc jade study
+lesson
mixin study(name, courses)
  p #{name} study
  ul.courses
    each course in courses
      li= course

h3 嵌套的 mixin
+study('tom', ['jade', 'node'])
mixin group(student)
  h4 #{student.name}
  +study(student.name, student.courses)
+group({name: 'tom', courses: ['jade', 'node']})

h3 mixin 的块包含
mixin team(slogon)
  h4 #{slogon}
  if block
    block
  else
    p no team
+team('slogon')
  p Good job!

h3 mixin 传递属性
mixin attr(name)
  p(class!=attributes.class) #{name}
+attr('attr')(class='magic')
mixin attrs(name)
  p&attributes(attributes) #{name}
+attrs('attrs')(class='magic2', id='attrid')

h3 mixin 传递位置个数参数
mixin magic(name, ...items)
  ul(class='#{name}')
   each item in items
    li= item
+magic('magic', 'node', 'jade', '..')
```
编译后：
```html
<h2>神奇的 mixins</h2>
  <p>imooc jade study</p>
<h3>嵌套的 mixin</h3>
  <p>tom study</p>
  <ul class="courses">
    <li>jade</li>
    <li>node</li>
  </ul>
  <h4>tom</h4>
    <p>tom study</p>
    <ul class="courses">
      <li>jade</li>
      <li>node</li>
    </ul>
<h3>mixin 的块包含</h3>
  <h4>slogon</h4>
  <p>Good job!</p>
<h3>mixin 传递属性</h3>
  <p class="magic">attr</p>
  <p id="attrid" class="magic2">attrs</p>
<h3>mixin 传递位置个数参数</h3>
  <ul class="magic">
    <li>node</li>
    <li>jade</li>
    <li>..</li>
  </ul>
```

# jade进阶

## 模板继承

```c
// layout.jade
doctype html
html
  head
    block scripts
      script(src='jquery.js')
    block styles
  body
    block content
      p there's no content here
```
等价于：
```
<doctype html>
<html>
  <head>
    <script src='jquery.js'></script>
  </head>
  <body>
    <p>there's no content here</p>
  </body>
</html>
```
```c
// page1.jade（假设和layout.jade相同路径）

extends layout // .jade扩展名可以省略

block scripts  // 替代原模板中的 block scripts
  script(src='jquery.js')
  script(src='underscore.js')

block content // 替代原模板中的 block content
```
等价于
```html
<doctype html>
<html>
  <head>
    <script src='jquery.js'></script>
    <script src='underscore.js'></script>
  </head>
  <body>
  </body>
</html>
```

## 模板包含

jade编写：
```jade
h3 包含
  include style
    style.
      h2 {
        color: #000;
      }
  include title.html
  block desc
    p desc from index
```

编译后：
```html
<h3>包含</h3>
  <style>
    body {
      color: #999;
    }
  </style>
  <style>
    h2 {
      color: #000;
    }
  </style>
  <div>
    <h3>content from html</h3>
  </div>
  <p>desc from index</p>
```


## 过滤器
jade编写：
```jade
h2 过滤器 filters
  h3 markdown
  :markdown
    Hi, this is **imooc** [link](http://imooc.com)
  h3 less
  style
    :less
      body {
        p {
          color: #ccc;
        }
      }

  h3 coffee
  script
    :coffee
      console.log 'This is coffee!'
```
编译后：
```html
<h2>过滤器 filters</h2>
  <h3>markdown</h3><p>Hi, this is <strong>imooc</strong> <a href="http://imooc.com">link</a></p>
  <h3>less</h3>
  <style>
    body p {
      color: #ccc;
  }
  </style>
  <h3>coffee</h3>
  <script>
    (function() {
      console.log('This is coffee!');
    }).call(this);
  </script>
```

## runtime环境下使用jade
jade --client --no-debug runtime.jade //命令
```jade
h2 浏览器端使用 jade
  #runtime
  script(src='node_modules/jade/runtime.js')
  script(src='runtime.js')
  script.
    var runtimeNode = document.getElementById('runtime');
    var runtimeHtml = template({isRuntime: true});
    runtimeNode.innerHTML = runtimeHtml;

    span 12
```
编译后：
```html
<h2>浏览器端使用 jade</h2>
  <div id="runtime"></div>
  <script src="node_modules/jade/runtime.js"></script>
  <script src="runtime.js"></script>
  <script>
    var runtimeNode = document.getElementById('runtime');
    var runtimeHtml = template({isRuntime: true});
    runtimeNode.innerHTML = runtimeHtml;
    
    span 12
  </script>
```

## 利用html2jade反编译
```c
html2jade http://twitter.com
html2jade http://twitter.com > twitter.jade
html2jade mywebpage.html # outputs mywebpage.jade
html2jade public/*.html  # converts all .html files to .jade
```




