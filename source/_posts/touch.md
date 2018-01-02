---
title: 移动端touch事件
categories: 前端学习笔记
tags: 移动端
comment: true
date: 2016-03-20 20:26:07
---
移动端touch事件总结

<!-- more -->

# 轮播图触摸事件
重点掌握touch事件对象的touches属性。
例如：e.touches[0].clientX
```
//规定偏移多少要进行翻页
var OFFSET = 50;
// 轮播图触摸
$('.carousel').each(function(i, item) {
  var startX, endX;
  item.addEventListener('touchstart', function(e) {
    // console.log(e);
    startX = e.touches[0].clientX;
    console.log(startX);
    console.log('start');
    e.preventDefault();
  });
  item.addEventListener('touchmove', function(e) {
    endX = e.touches[0].clientX;
    console.log(endX);

    e.preventDefault();
  });
  item.addEventListener('touchend', function(e) {
    console.log('over');
    var offsetX = endX - startX;
    console.log(offsetX);
    if (offsetX > OFFSET) {
      // 上一张
      $(this).carousel('prev');
    } else if (offsetX < -OFFSET) {
      // 上一张
      $(this).carousel('next');
    }
    e.preventDefault();
  });
});
```
# 实例
```
<body>
  <p id="desc"></p>
  <div id="touchPad" class="touchpad">触摸板</div>
  <div id="ball" class="ball"></div>
  <script src="../js/zepto.js"></script>
  <script type="text/javascript">
  var touchpad = document.querySelector('#touchPad'),
    ball = document.querySelector('#ball'),
    desc = document.querySelector('#desc');


  //获取touch的点(兼容mouse事件)
  var getTouchPos = function(e) {
    var touches = e.touches;
    if (touches && touches[0]) {
      return {
        x: touches[0].clientX,
        y: touches[0].clientY
      };
    }
    return {
      x: e.clientX,
      y: e.clientY
    };
  }

  //计算两点之间距离
  var getDist = function(p1, p2) {
      if (!p1 || !p2) return 0;
      return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
    }
    //计算两点之间所成角度
  var getAngle = function(p1, p2) {
    var r = Math.atan2(p2.y - p1.y, p2.x - p1.x);
    var a = r * 180 / Math.PI;
    return a;
  };
  //获取swipe的方向
  var getSwipeDirection = function(p2, p1) {
    var dx = p2.x - p1.x;
    var dy = -p2.y + p1.y;
    var angle = Math.atan2(dy, dx) * 180 / Math.PI;

    if (angle < 45 && angle > -45) return "right";
    if (angle >= 45 && angle < 135) return "top";
    if (angle >= 135 || angle < -135) return "left";
    if (angle >= -135 && angle <= -45) return "bottom";

  }


  var startEvtHandler = function(e) {
    var pos = getTouchPos(e);
    ball.style.left = pos.x + 'px';
    ball.style.top = pos.y + 'px';
    ball.style.display = 'block';

    var touches = e.touches;
    if (!touches || touches.length == 1) { //touches为空，代表鼠标点击
      point_start = getTouchPos(e);
      time_start = Date.now();
    }
  }

  var moveEvtHandler = function(e) {
    var pos = getTouchPos(e);
    ball.style.left = pos.x + 'px';
    ball.style.top = pos.y + 'px';


    point_end = getTouchPos(e);
    e.preventDefault();
  }

  //touchend的touches和targetTouches没有对象，只有changeTouches才有
  var endEvtHandler = function(e) {
    ball.style.display = 'none';

    var time_end = Date.now();

    //距离和时间都符合
    if (getDist(point_start, point_end) > SWIPE_DISTANCE && time_start - time_end < SWIPE_TIME) {

      var dir = getSwipeDirection(point_end, point_start);
      touchPad.innerHTML = 'swipe' + dir;
    }
  }


  var SWIPE_DISTANCE = 30; //移动30px之后才认为swipe事件
  var SWIPE_TIME = 500; //swipe最大经历时间
  var point_start,
    point_end,
    time_start,
    time_end;

  //判断是PC或者移动设备
  var startEvt, moveEvt, endEvt;
  if ("ontouchstart" in window) {
    startEvt = "touchstart";
    moveEvt = "touchmove";
    endEvt = "touchend";
  } else {
    startEvt = "mousedown";
    moveEvt = "mousemove";
    endEvt = "mouseup";
  }

  touchpad.addEventListener(startEvt, startEvtHandler);
  touchpad.addEventListener(moveEvt, moveEvtHandler);
  touchpad.addEventListener(endEvt, endEvtHandler);
  </script>
</body>
```
