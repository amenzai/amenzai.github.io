---
title: 响应式图片解决方案
categories: 前端学习笔记
tags: responsive
comment: true
date: 2016-05-20 20:26:07
---

响应式图片实现方法：js、服务器、srcset、picture、svg 具体实现可以用picture加腻子脚本。

<!-- more -->

# picture加腻子脚本
```
<!-- 先引入 -->
<script src="lib/picturefill.min.js"></script>

<div class="item">
    <picture>
        <source srcset="img/ad001-l.png" media="(min-width:50em)">
        <source srcset="img/ad001-m.png" media="(min-width:30em)">
        <img src="img/ad001.png" alt="2015年度报告">
    </picture>
</div>
<div class="item">
    <picture>
        <source srcset="img/ad002-l.png" media="(min-width: 50em)">
        <source srcset="img/ad002-m.png" media="(min-width: 30em)">
        <img srcset="img/ad002.png" alt="新年红包">
    </picture>
</div>
```
# js判断窗口尺寸
```
function resize() {
  // 屏幕宽度
  var windowWidth = $(window).width();
  // 是否为小于768的屏幕
  var smallScreen = windowWidth < 768;
  // 轮播图板块适应
  var $itemImages = $('#home_slide .item-image');
  $itemImages.each(function(i, item) {
    var $item = $(item);
    var imgSrc = $item.data(smallScreen ? 'image-small' : 'image-large');
    var imgAlt = $item.data('image-alt');
    $item.html('<img src="' + imgSrc + '" alt="' + imgAlt + '"/>');
    $item.css('backgroundImage', 'url(' + imgSrc + ')');
  });
}

```