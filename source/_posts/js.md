---
title: js常用代码块
categories: 前端学习笔记
tags: [js代码段,javascript]
comment: true
date: 2016-01-10 20:26:07
---
js常用代码块整理。

<!-- more -->

# 事件对象兼容写法
```javascript
document.onclick = function(event) { // 文档中点击
  var event = event || window.event; // 兼容性写法
  console.log(event.clientY);
  console.log(event.pageY);
  console.log(event.screenY);
}
```
# 一个盒子内的坐标
```javascript
var div = document.getElementsByTagName("div")[0];
div.onmousemove = function(event) {
  var event = event || window.event;
  var x = event.clientX - this.offsetLeft;
  var y = event.clientY - this.offsetTop;
  this.innerHTML = x + "px" + y + "px";
}
```

# 封装scrollTop Left函数
```javascript
function $(id) {
  return document.getElementById(id);
}

function show(obj) { obj.style.display = "block"; }

function hide(obj) { obj.style.display = "none"; }

function scroll() {
  if (window.pageYOffset != null) //  ie9+ 和其他浏览器
  {
    return {
      left: window.pageXOffset,
      top: window.pageYOffset
    }
  } else if (document.compatMode == "CSS1Compat") // 声明的了 DTD
  // 检测是不是怪异模式的浏览器 -- 就是没有 声明<!DOCTYPE html>
  {
    return {
      left: document.documentElement.scrollLeft,
      top: document.documentElement.scrollTop
    }
  }
  return { //  剩下的肯定是怪异模式的
    left: document.body.scrollLeft,
    top: document.body.scrollTop
  }
}
window.onscroll = function() {
  console.log(scroll().top);
}
```

# 导航固定写法
```javascript
var nav = $("Q-nav");
var navTop = nav.offsetTop; // 得到导航栏距离顶部的距离  168
console.log(navTop);
window.onscroll = function() {
  // console.log(nav.offsetTop);
  if (scroll().top >= navTop) {
    //alert("到位置了");
    nav.className = "nav fixed";
  } else {
    nav.className = "nav";
  }
}
```

# 两边跟随的广告（广告图绝对定位）
```javascript
window.onload = function() {
  var pic = $("pic");
  var leader = 0;
  var target = 0;
  var timer = null; // 定时器
  var top = pic.offsetTop; // 50
  window.onscroll = function() {
    clearInterval(timer);
    target = scroll().top + top; // 把最新的 scrolltop 给  target
    timer = setInterval(function() {
      leader = leader + (target - leader) / 10;
      pic.style.top = leader + 'px';
    }, 30)
  }
}
```

# 返回顶部的小火箭
```
window.onload = function() {
  var goTop = $("gotop");
  window.onscroll = function() {
    scroll().top > 0 ? show(goTop) : hide(goTop); // 如果大于0 就显示 否则隐藏
    leader = scroll().top; // 把 卷去的头部 给  起始位置
    console.log(scroll().top);
  }
  var leader = 0,
    target = 0,
    timer = null;
  // leader 起始位置  target  目标位置
  goTop.onclick = function() {
    target = 0; //  点击完毕之后 奔向0 去的  不写也可以
    timer = setInterval(function() {
      leader = leader + (target - leader) / 10;
      window.scrollTo(0, leader); // 去往页面中的某个位置
      if (leader == target) {
        clearInterval(timer);
      }
    }, 20);

  }
}
```

# 三个取整函数
```
console.log(Math.ceil(1.01))
console.log(Math.ceil(1.9))
console.log(Math.ceil(-1.3))
  //floor  地板   向下取整
console.log(Math.floor(1.01))
console.log(Math.floor(1.9))
console.log(Math.floor(-1.3))
  // 四舍五入
console.log(Math.round(1.01))
console.log(Math.round(1.5))
```

# 封装可视区域大小函数（窗口宽度）
```
function client() {
  if (window.innerWidth != null) // ie9 +  最新浏览器
  {
    return {
      width: window.innerWidth,
      height: window.innerHeight
    }
  } else if (document.compatMode === "CSS1Compat") // 标准浏览器
  {
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    }
  }
  return { // 怪异浏览器
    width: document.body.clientWidth,
    height: document.body.clientHeight

  }
}
// document.write(client().width);

window.onresize = reSize;

function reSize() {
  var clientWidth = client().width;
  if (clientWidth > 980) {
    styleCss.href = "";
  } else if (clientWidth > 640) {
    styleCss.href = "css/pad.css";
  } else {
    styleCss.href = "css/mobile.css";
  }
}
//检测屏幕分辨率
document.write(window.screen.width);
```

# 阻止冒泡方法
```
var btn = document.getElementById("btn");
document.onclick = function() {
  alert("点击了空白处")
}
btn.onclick = function(event) {
  alert("点击了按钮")
  var event = event || window.event;
  if (event && event.stopPropagation) {
    event.stopPropagation(); //  w3c 标准
  } else {
    event.cancelBubble = true; // ie 678  ie浏览器
  }
}
```

# 点击空白隐藏
```
function $(id) {
  return document.getElementById(id);
}
var login = document.getElementById("login");
login.onclick = function(event) {
  $("mask").style.display = "block";
  $("show").style.display = "block";
  document.body.style.overflow = "hidden"; // 不显示滚动条
  //取消冒泡
  var event = event || window.event;
  if (event && event.stopPropagation) {
    event.stopPropagation();
  } else {
    event.cancelBubble = true;
  }
}
document.onclick = function(event) {

  var event = event || window.event;
  // alert(event.target.id);  // 返回的是点击的某个对象的id 名字
  // alert(event.srcElement.id);
  var targetId = event.target ? event.target.id : event.srcElement.id;
  // 看明白这个写法
  if (targetId != "show") // 不等于当前点点击的名字
  {
    $("mask").style.display = "none";
    $("show").style.display = "none";
    document.body.style.overflow = "visible"; // 显示滚动条
  }
}
```

# 返回元素当前样式
```
var demo = document.getElementById("demo");

function getStyle(obj, attr) { //  谁的      那个属性
  if (obj.currentStyle) // ie 等
  {
    return obj.currentStyle[attr]; // 返回传递过来的某个属性
  } else {
    return window.getComputedStyle(obj, null)[attr]; // w3c 浏览器
  }
}

console.log(getStyle(demo, "width"));
```

# 闭包版tab栏 节流
```
window.onload = function() {
  //要想多个盒子不相互影响 ，我们可以通过id 给他们分开
  //封装tab栏切换函数
  function tab(obj) {
    var target = document.getElementById(obj);
    var spans = target.getElementsByTagName("span");
    var lis = target.getElementsByTagName("li");
    for (var i = 0; i < spans.length; i++) {
      //  spans[i].index = i;
      spans[i].onmouseover = function(num) {
        return function() {
          for (var j = 0; j < spans.length; j++) {
            spans[j].className = "";
            lis[j].className = "";
          }
          spans[num].className = "current";
          lis[num].className = "show";
        }
      }(i);


    }
  }
  tab("one");
  tab("two");
  tab("three");
}
```

# 窗口尺寸改变，触发不那么灵敏 闭包版本
```
var num = 0;
var demo = document.getElementById("demo")
window.onresize = throttle(function() {
  demo.innerHTML = window.innerWidth || document.documentElement.clientWidth;
  num++;
  console.log(num);
}, 300);

function throttle(fn, delay) { // 闭包  节流
  var timer = null;
  return function() {
    clearTimeout(timer);
    timer = setTimeout(fn, delay);
  }
}
```

# 运动函数
```
var btn200 = document.getElementById("btn200");
var btn400 = document.getElementById("btn400");
var box = document.getElementById("box");
btn200.onclick = function() {
  animate(box, { width: 200, top: 100, left: 200, opacity: 40, zIndex: 3 }, function() { alert("我来了") });
}
btn400.onclick = function() {
    animate(box, { top: 500 });
  }
  // 多个属性运动框架  添加回调函数
function animate(obj, json, fn) { // 给谁    json
  clearInterval(obj.timer);
  obj.timer = setInterval(function() {
    var flag = true; // 用来判断是否停止定时器   一定写到遍历的外面
    for (var attr in json) { // attr  属性     json[attr]  值
      //开始遍历 json
      // 计算步长    用 target 位置 减去当前的位置  除以 10
      // console.log(attr);
      var current = 0;
      if (attr == "opacity") {
        current = Math.round(parseInt(getStyle(obj, attr) * 100)) || 0;
        console.log(current);
      } else {
        current = parseInt(getStyle(obj, attr)); // 数值
      }
      // console.log(current);
      // 目标位置就是  属性值
      var step = (json[attr] - current) / 10; // 步长  用目标位置 - 现在的位置 / 10
      step = step > 0 ? Math.ceil(step) : Math.floor(step);
      //判断透明度
      if (attr == "opacity") // 判断用户有没有输入 opacity
      {
        if ("opacity" in obj.style) // 判断 我们浏览器是否支持opacity
        {
          // obj.style.opacity
          obj.style.opacity = (current + step) / 100;
        } else { // obj.style.filter = alpha(opacity = 30)
          obj.style.filter = "alpha(opacity = " + (current + step) * 10 + ")";

        }
      } else if (attr == "zIndex") {
        obj.style.zIndex = json[attr];
      } else {
        obj.style[attr] = current + step + "px";
      }

      if (current != json[attr]) // 只要其中一个不满足条件 就不应该停止定时器  这句一定遍历里面
      {
        flag = false;
      }
    }
    if (flag) // 用于判断定时器的条件
    {
      clearInterval(obj.timer);
      //alert("ok了");
      if (fn) // 很简单   当定时器停止了。 动画就结束了  如果有回调，就应该执行回调
      {
        fn(); // 函数名 +  （）  调用函数  执行函数 暂且这样替代
      }
    }
  }, 30)
}
```

# 获取元素样式中的某个属性
```
function getStyle(obj, attr) { //  谁的      那个属性
  if (obj.currentStyle) // ie 等
  {
    return obj.currentStyle[attr]; // 返回传递过来的某个属性
  } else {
    return window.getComputedStyle(obj, null)[attr]; // w3c 浏览器
  }
}
```
# 全选和反选
```
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
```
# tab1栏切换
```
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
```
# 定时器应用
```
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
# 定时器应用
```
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
# 无缝滚动
```
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
```
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
```
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
```
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
