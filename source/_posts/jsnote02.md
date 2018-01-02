---
title: javascript代码笔记02
categories: 前端学习笔记
tags: [js代码段,javascript]
comment: true
date: 2015-12-6 20:26:07
---
整理的一些javascript代码段，使用原生的js编写

<!-- more -->

# 仿微博内容发布
使用了一些DOM节点的操作以及节点之间的关系。难点：每次发布的内容要位于第一条。

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        ul{
            list-style-type: none;}
        *{margin:0;padding: 0;}
        .box {
            margin: 100px auto;
            width: 600px;
            height: auto;
            border:1px solid #333;
            padding: 30px 0;
        }
        textarea:focus,button:focus {
      outline: none;
        }
        textarea {
            width: 450px;
            resize: none;  /*防止用户拖动 右下角*/
            display: block;
            margin: 20px auto;
        }
        .box li {
            width: 450px;
            line-height: 30px;
            border-bottom:1px dashed #ccc;
            margin-left: 80px;
        }
        .box li a {
            float: right;
        }
    </style>
    <script>
        window.onload = function(){
            //获取对象   再次操作对象
            var btn = document.getElementsByTagName("button")[0];
            var txt = document.getElementsByTagName("textarea")[0];
            var ul = document.createElement("ul");
            btn.parentNode.appendChild(ul);
            btn.onclick = function(){
                if(txt.value == "")
                {
                    alert("输入不能为空");
                    return false;
                }
                var newli = document.createElement("li");
                newli.innerHTML = txt.value + "<a href ='javascript:;'>删除</a>";

                txt.value = "";
                var lis = ul.children;
                if(lis.length == 0)
                {
                    ul.appendChild(newli);
                }
                else
                {
                    ul.insertBefore(newli,lis[0]);
                }
                var as = document.getElementsByTagName("a");
                for(var i=0; i<as.length;i++)
                {
                    as[i].onclick = function(){
                        ul.removeChild(this.parentNode);
                    }
                }
            }
     }
    </script>
</head>
<body>
<div class="box">
    微博发布: <br>
    <textarea name="" id="" cols="30" rows="10"></textarea>
    <button>发布</button>
</div>
</body>
</html>
```

# 定时器应用

- 简单的定时器进行页面元素的显示隐藏，给页面元素加相应id即可生效
```javascript

  function $(id) { return document.getElementById(id);}  // id函数
  function hide(id) {   // 隐藏函数
      $(id).style.display = "none";
  }
  function show(id) {
      $(id).style.display = "block";
  }
  setTimeout(closeAd,5000);
  function closeAd() {
      hide("left");
      hide("right");
  }

```

- 鼠标移动到图片上部图片向上滑动，鼠标移动到图片下部图片向下滑动
```javascript


  <div class="xiaomi">
    <!-- 这里放一个超高的图片，需要进行绝对定位，超出的部分，让父亲隐藏掉。 -->
      <img src="mi.png" alt="" id="pic"/>
      <!-- 上下部分触发事件的绝对定位块 -->
      <span class="up" id="picUp"></span>
      <span class="down" id="picDown"></span>
  </div>

  function $(id) { return document.getElementById(id);}
  var num = 0; // 控制图片的top值
  var timer = null; // 定时器名称
  $("picUp").onmouseover = function(){
      clearInterval(timer);
      timer =  setInterval(function(){
           num -= 3;
           //这里的数值都是根据给定的图片高度算出的
           num >= -1070 ? $("pic").style.top = num + "px" :  clearInterval(timer);

       },30);
  }
  $("picDown").onmouseover = function(){
      clearInterval(timer);
        timer  = setInterval(function(){
            num+=3;
           num < 0 ?  $("pic").style.top = num + "px" : clearInterval(timer);
        },30);
  }
  $("picUp").parentNode.onmouseout = function() {
      clearInterval(timer);
  }

```

- 无缝滚动

```javascript

  var scroll = document.getElementById("scroll");
    var ul = scroll.children[0];
    var num = 0; //控制左侧值  left
    var timer = null;  // 定时器
    timer = setInterval(autoPlay,20);
    function autoPlay() {
        num--;
        num<=-1200 ? num = 0 : num;
        ul.style.left = num + "px";
    }
    scroll.onmouseover = function() {  // 鼠标经过大盒子  停止定时器
        clearInterval(timer);
    }
    scroll.onmouseout = function() {
        timer = setInterval(autoPlay,20);  // 开启定时器
    }

```

# 检测字符串长度

```javascript

  var txt = "what are you 弄啥咧!好的";
    console.log(txt.length);
    function getStringLength(str) {
        var len = 0; //存储总长度
        var c = 0;  // 存储每一个编码
        for(var i=0;i<str.length;i++)
        {
            c = str.charCodeAt(i);
            if(c>=0 && c<=127)
            {
                len++;
            }
            else
            {
                len += 2;
            }
        }
        return len;
    }

    console.log(getStringLength(txt));

```

# 封装自己的$函数，快速获取页面元素

```javascript

   //function $(id) {return document.getElementById(id)}  //id选择器

   function getClass(classname)   //  类的写法
   {
       //判断支持否
       if(document.getElementsByClassName)
       {
           return document.getElementsByClassName(classname);
       }
       var arr = []; //用于返回 数组
       var dom = document.getElementsByTagName("*");
       for(var i=0;i<dom.length;i++)  // 遍历所有的 盒子
       {
           var txtarr = dom[i].className.split(" "); // 分割类名 并且 转换为数组
           //  ["demo","test"];
           for(var j=0;j<txtarr.length;j++)  // 遍历 通过类名分割的数组
           {
               if(txtarr[j] == classname)
               {
                   arr.push(dom[i]); // 我们要的是 div
               }
           }
       }
       return arr;
   }

   // $("#demo")   $(".one")   $("div")
    //封装自己的$
    function $(str) {
        var s = str.charAt(0);   //  一个s 的变量 存放是 符号  #  .
        var ss = str.substr(1);  // demo
        switch(s)
        {
            case "#":
                return document.getElementById(ss);
            break;
            case ".":
                return getClass(ss);
            break;
            default :
                return document.getElementsByTagName(str);
        }
    }


  $("#demo").style.backgroundColor = "purple";
  $("#last").style.backgroundColor = "purple";
   var test = $(".one");
    for(var i=0;i<test.length;i++)
    {
        test[i].style.backgroundColor = "blue";
    }

```

# 匀速运动与缓动动画

```javascript
    //匀速运动
    var btn = document.getElementById("btn");
      var box = document.getElementById("bOX");
      var num = 0;
      var timer = null;
      btn.onclick = function() {
          timer = setInterval(function(){
              num++;
              if(num >=500)
              {
                  clearInterval(timer);
              }
              else
              {
                  box.style.left = num + "px";
              }
          },10)
      }

      //缓动动画
      var btn = document.getElementById("btn");
      var box = document.getElementById("bOX");
      var timer = null;
      var leader = 0;
      var target = 500;
      btn.onclick = function() {
          setInterval(function(){
              leader = leader + (target - leader )/10;
              box.style.left = leader + "px";
          },30)
      }

```
