---
title: javascript代码笔记01
categories: 前端学习笔记
tags: [js代码段,javascript]
comment: true
date: 2015-11-30 20:26:07
---

整理的一些javascript代码段，使用原生的js编写

<!-- more -->

## 全选和反选
使用到js入口函数、函数的封装、js事件的用法、for循环遍历数组。

```html
<!DOCTYPE html>
<html>
<head lang="zh-CN">
    <meta charset="UTF-8">
    <title>全选和反选</title>
    <script>
        window.onload = function(){
            var btns = document.getElementsByTagName("button");
            var inputs = document.getElementById("bottom").getElementsByTagName("input");
            /*全选和取消 函数*/
             function all(flag){
                 for(var i=0;i<inputs.length;i++)
                 {
                     inputs[i].checked = flag;
                 }
             }
             btns[0].onclick = function(){
                  all(true);
             };
             btns[1].onclick = function(){
                  all(false);
             };
             btns[2].onclick = function(){

                 for(var i=0;i<inputs.length;i++)
                 {
                     inputs[i].checked == true ?  inputs[i].checked = false : inputs[i].checked = true;
                 }
             }
        }
    </script>
</head>
<body>
<div id="top">
    <button>全选</button>
    <button>取消</button>
    <button>反选</button>
</div>
<div id="bottom">
    <ul>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
        <li>选项: <input type="checkbox"/></li>
    </ul>
</div>
</body>
</html>
```

## tab栏切换

编写难点在于排他思想的使用以及给元素添加属性，如果在页面中需要用到多个tab栏，我们就要对每个tab栏外面定义一个id盒子，这些在源码中都注释的有。

```html
<!DOCTYPE html>
<html>
<head lang="zh-CN">
    <meta charset="UTF-8">
    <title>tab栏切换</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        button:focus {
            outline: none;
        }
        button {
            cursor: pointer;
        }
        ul {
            list-style: none;
        }
        .box {
            width: 400px;
            margin:100px auto;
            border:1px solid #ccc;
        }
        .bottom li{
            width:100%;
            height: 300px;
            background-color: #f5f5f5;
            display: none;
        }
        .bg{
            background-color: #333;
            color: #fff;
        }
        .bottom .show {
            display: block;
        }

    </style>
    <script>
        window.onload = function(){
            function tab(obj) {
                console.log(222);
                var obj = document.getElementById(obj);
                var btns = obj.getElementsByTagName('button');
                var lis= obj.getElementsByTagName('li');
                for (var i = 0; i < btns.length; i++) {
                    btns[i].index = i; //难点
                    btns[i].onmouseover = function() {
                        for (var j = 0; j < btns.length; j++) {
                            //让所有的 btn 类名清空
                            btns[j].className = "";
                            lis[j].className = "";
                            // 当前的那个按钮 的添加 类名
                            this.className = "bg"
                            //先隐藏下面所有的 li盒子
                            //留下中意的那个 跟点击的序号有关系的
                            lis[this.index].className = "show";
                        }
                    }
                }
            }
            tab("one");
            tab("two");
        }
    </script>
</head>
<body>
<div class="box" id="one">
    <div>
        <button class="bg">第一个</button>
        <button>第二个</button>
        <button>第三个</button>
        <button>第四个</button>
        <button>第五个</button>
    </div>
    <ul class="bottom">
        <li class="show">1好盒子</li>
        <li>2好盒子</li>
        <li>3好盒子</li>
        <li>4好盒子</li>
        <li>5好盒子</li>
    </ul>
</div>
<div class="box" id="two">
    <div class="top">
        <button class="bg">第一个</button>
        <button>第二个</button>
        <button>第三个</button>
        <button>第四个</button>
        <button>第五个</button>
    </div>
    <ul class="bottom">
        <li class="show">1好盒子</li>
        <li>2好盒子</li>
        <li>3好盒子</li>
        <li>4好盒子</li>
        <li>5好盒子</li>
    </ul>
</div>
</body>
</html>
```
