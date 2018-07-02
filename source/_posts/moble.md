---
title: 移动端笔记（meta标签）
categories: 前端学习笔记
tags: 移动端
comment: true
date: 2016-03-01 20:26:07
---
移动端常用meta标签整理

<!-- more -->

# 百度禁止转码
通过百度手机打开网页时，百度可能会对你的网页进行转码，往你页面贴上它的广告，非常之恶心。不过我们可以通过这个meta标签来禁止它：
```html
<meta http-equiv="Cache-Control" content="no-siteapp" />
```

# 添h加到主屏后的标题（IOS）
```html
<meta name="apple-mobile-web-app-title" content="标题">
```

# 启用 WebApp 全屏模式（IOS）
当网站添加到主屏幕后再点击进行启动时，可隐藏地址栏（从浏览器跳转或输入链接进入并没有此效果）
```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-touch-fullscreen" content="yes" />
```
PS：然而，经本人用5S测试，设置”apple-touch-fullscreen”并没有什么卵用，希望了解者能在底部评论告知

# 设置状态栏的背景颜色（IOS）
设置状态栏的背景颜色，只有在 `name=”apple-mobile-web-app-capable” content=”yes”` 时生效
```html
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```
上面content 参数说明：

- default ：状态栏背景是白色。
- black ：状态栏背景是黑色。
- black-translucent ：状态栏背景是半透明。 如果设置为 default 或 black ,网页内容从状态栏底部开始。 如果设置为 black-translucent ,网页内容充满整个屏幕，顶部会被状态栏遮挡
。
# 移动端手机号码识别（IOS）
在 iOS Safari （其他浏览器和Android均不会）上会对那些看起来像是电话号码的数字处理为电话链接，比如：7位数字，形如：1234567。带括号及加号的数字，形如：(+86)123456789。双连接线的数字，形如：00-00-00111
11位数字，形如：13800138000。

可能还有其他类型的数字也会被识别。我们可以通过如下的meta来关闭电话号码的自动识别：
```html
<meta name="format-detection" content="telephone=no" />
```
但某些时候，你关闭电话自动识别后，又希望某些电话号码长按时能够链接到 iPhone 的拨号功能和短信功能，你可以使用下面的方法实现：

开启电话功能：
```html
 <a href=”tel:123456″>123456</a> 
```

开启短信功能：
```html
 <a href=”sms:123456″>123456</a> 
```

# 移动端邮箱识别（Android）
与电话号码的识别一样，在安卓上会对符合邮箱格式的字符串进行识别，我们可以通过如下的meta来管别邮箱的自动识别：
```html
 <meta content=”email=no” name=”format-detection” />
``` 
同样地，我们也可以通过标签属性来开启长按邮箱地址弹出邮件发送的功能：
```html
<a href="mailto:dooyoe@gmail.com">dooyoe@gmail.com</a>
```

# 添加智能 App 广告条 Smart App Banner（IOS 6+ Safari）
```html
 <meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”> 
```

# IOS Web app启动动画
由于iPad 的启动画面是不包括状态栏区域的。所以启动图片需要减去状态栏区域所对应的方向上的20px大小，相应地在retina设备上要减去40px的大小
```html
<!-- iPhone -->
<link href="apple-touch-startup-image-320x460.png" media="(device-width: 320px)" rel="apple-touch-startup-image">
<!-- iPhone (Retina) -->
<link href="apple-touch-startup-image-640x960.png" media="(device-width: 320px) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
<!-- iPad (portrait) -->
<link href="apple-touch-startup-image-768x1004.png" media="(device-width: 768px) and (orientation: portrait)" rel="apple-touch-startup-image">
<!-- iPad (landscape) -->
<link href="apple-touch-startup-image-748x1024.png" media="(device-width: 768px) and (orientation: landscape)" rel="apple-touch-startup-image">
<!-- iPad (Retina, portrait) -->
<link href="apple-touch-startup-image-1536x2008.png" media="(device-width: 1536px) and (orientation: portrait) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
<!-- iPad (Retina, landscape) -->
<link href="apple-touch-startup-image-2048x1496.png" media="(device-width: 1536px)  and (orientation: landscape) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
（landscape：横屏 | portrait：竖屏）
```

# 添加到主屏后的APP图标
指定web app添加到主屏后的图标路径，有两种略微不同的方式：
```html
<!-- 设计原图 -->
<link href="short_cut_114x114.png" rel="apple-touch-icon-precomposed">
<!-- 添加高光效果 -->
<link href="short_cut_114x114.png" rel="apple-touch-icon">
```
上面的rel参数说明：

- apple-touch-icon：在IOS6及以下的版本会自动为图标添加一层高光效果（IOS7开始已使用扁平化的设计风格）
- apple-touch-icon-precomposed：使用"设计原图图标"


可通过指定size属性来为不同的设备提供不同的图标（但通常来说，我们只需提供一个114 x 114 pixels大小的图标即可 ）

# 优先使用最新版本 IE 和 Chrome
```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
```

# 关闭iOS键盘首字母自动大写
在iOS中，默认情况下键盘是开启首字母大写的功能的，如果禁用这个功能，可以这样：
```html
<input type="text" autocapitalize="off" />
```

# 关闭iOS输入自动修正
和英文输入默认自动首字母大写那样，IOS还做了一个功能，默认输入法会开启自动修正输入内容，这样的话，用户经常要操作两次。如果不希望开启此功能，我们可以通过input标签属性来关闭掉：
```html
<input type="text" autocorrect="off" />
```

# 禁止文本缩放
当移动设备横竖屏切换时，文本的大小会重新计算，进行相应的缩放，当我们不需要这种情况时，可以选择禁止：
```css
html {
　　-webkit-text-size-adjust: 100%;
}
```
需要注意的是，PC端的该属性已经被移除，该属性在移动端要生效，必须设置 `meta viewport’。

# 移动端如何清除输入框内阴影
在iOS上，输入框默认有内部阴影，但无法使用 box-shadow 来清除，如果不需要阴影，可以这样关闭：
```css
input,
textarea {
　　border: 0; /* 方法1 */
　　-webkit-appearance: none; /* 方法2 */
}
```

# 快速回弹滚动
我们先来看看回弹滚动在手机浏览器发展的历史：

早期的时候，移动端的浏览器都不支持非body元素的滚动条，所以一般都借助 iScroll;

Android 3.0/iOS解决了非body元素的滚动问题，但滚动条不可见，同时iOS上只能通过2个手指进行滚动；

Android 4.0解决了滚动条不可见及增加了快速回弹滚动效果，不过随后这个特性又被移除；

iOS从5.0开始解决了滚动条不可见及增加了快速回弹滚动效果。

在iOS上如果你想让一个元素拥有像 Native 的滚动效果，你可以这样做：
```css
.xxx {
    overflow: auto; /* auto | scroll */
    -webkit-overflow-scrolling: touch;
}
```
PS：iScroll用过之后感觉不是很好，有一些诡异的bug，这里推荐另外一个 iDangero Swiper，这个插件集成了滑屏滚动的强大功能（支持3D），而且还有回弹滚动的内置滚动条，官方地址：
```
http://www.idangero.us/sliders/swiper/index.php
```

# 移动端禁止选中内容
如果你不想用户可以选中页面中的内容，那么你可以在css中禁掉：
```css
.user-select-none {
  -webkit-user-select: none;  /* Chrome all / Safari all */
  -moz-user-select: none;     /* Firefox all （移动端不需要） */
  -ms-user-select: none;      /* IE 10+ */      
}
```

# 移动端取消touch高亮效果
在做移动端页面时，会发现所有a标签在触发点击时或者所有设置了伪类 :active 的元素，默认都会在激活状态时，显示高亮框，如果不想要这个高亮，那么你可以通过css以下方法来进行全局的禁止：
```css
html {
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
```
但这个方法在三星的机子上无效，有一种妥协的方法是把页面非真实跳转链接的a标签换成其它标签，可以解决这个问题。

# 如何禁止保存或拷贝图像（IOS）
通常当你在手机或者pad上长按图像 img ，会弹出选项 存储图像 或者 拷贝图像，如果你不想让用户这么操作，那么你可以通过以下方法来禁止：
```css
 img { -webkit-touch-callout: none; }
```

# calc用法
```html
<div class="calc">我是测试calc</div>

.calc{
    margin-left:50px;
    padding-left:2rem;
    width:calc(100%-50px-2rem);
    height:10rem;
}
```