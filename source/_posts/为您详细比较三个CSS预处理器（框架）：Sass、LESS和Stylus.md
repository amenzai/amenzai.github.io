---
title: 为您详细比较三个CSS预处理器（框架）：Sass、LESS和Stylus
comment: true
date: 2017-04-13 09:53:00
categories: 前端学习笔记
---
CSS 预处理器技术已经非常的成熟，而且也涌现出了越来越多的 CSS 的预处理器框架。本文向你介绍使用最为普遍的三款 CSS 预处理器框架，分别是 Sass、Less CSS、Stylus。

<!-- more -->

首先我们来简单介绍下什么是 CSS 预处理器，CSS 预处理器是一种语言用来为 CSS 增加一些编程的的特性，无需考虑浏览器的兼容性问题，例如你可以在 CSS 中使用变量、简单的程序逻辑、函数等等在编程语言中的一些基本技巧，可以让你的 CSS 更见简洁，适应性更强，代码更直观等诸多好处。

不要再停留在石器时代了，下面让我们开始 CSS 预处理器之旅。

我们将会从语法、变量、嵌套、混入(Mixin)、继承、导入、函数和操作符等方面分别对这三个框架进行比较介绍。

# 语法

在使用 CSS 预处理器之前最重要的是理解语法，幸运的是基本上大多数预处理器的语法跟 CSS 都差不多。

首先 Sass 和 Less 都使用的是标准的 CSS 语法，因此你可以很方便的将已有的 CSS 代码转为预处理器代码，默认 Sass 使用 .sass 扩展名，而 Less 使用 .less 扩展名。

## Less与Sass语法
```css
h1 {
  color: #0982C1;
}
```
你注意到了，这是一个再普通不过的，不过 Sass 同时也支持老的语法，就是不包含花括号和分号的方式：
```css
h1
  color: #0982c1
```
## Stylus语法

Stylus 支持的语法要更多样性一点，它默认使用 .styl 的文件扩展名，下面是 Stylus 支持的语法：
```css
h1 {
  color: #0982C1;
}

h1
  color: #0982C1;  

h1
  color #0982C1
```
你也可以在同一个样式单中使用不同的变量，例如下面的写法也不会报错：
```css
h1 {
  color #0982c1
}
h2
  font-size: 1.2em
```

# 变量

你可以在 CSS 预处理器中声明变量，并在整个样式单中使用，支持任何类型的变量，例如颜色、数值（不管是否包括单位）、文本。然后你可以任意引用该变量。

## Sass变量

Sass 的变量必须是 $ 开始，然后变量名和值使用冒号隔开，跟 CSS 的属性一致：

```css
$mainColor: #0982c1;
$siteWidth: 1024px;
$borderStyle: dotted;
  
body {
  color: $mainColor;
  border: 1px $borderStyle $mainColor;
  max-width: $siteWidth;
}
```

## Less变量
Less 的变量名使用 @ 符号开始：
```css
@mainColor: #0982c1;
@siteWidth: 1024px;
@borderStyle: dotted;
  
body {
  color: @mainColor;
  border: 1px @borderStyle @mainColor;
  max-width: @siteWidth;
}
```
## Stylus变量

Stylus 对变量名没有任何限定，你可以是 $ 开始，也可以是任意的字符，而且与变量值之间可以用冒号、空格隔开，需要注意的是 Stylus (0.22.4) 将会编译 @ 开始的变量，但其对应的值并不会赋予该变量，换句话说，在 Stylus 的变量名不要用 @ 开头。
```css
mainColor = #0982c1
siteWidth = 1024px
$borderStyle = dotted
  
body
  color mainColor
  border 1px $borderStyle mainColor
  max-width siteWidth
```
上面的三种不同的 CSS 预处理器的写法，最终都将产生相同的结果：

```css
body {
  color: #0982c1;
  border: 1px dotted #0982c1;
  max-width: 1024px;
}
```
你可以想象，加入你的 CSS 中使用了某个颜色的地方多达数十次，那么要修改颜色时你必须找到这数十次的地方并一一修改，而有了 CSS 预处理器，修改一个地方就够了！

# 嵌套
如果我们需要在CSS中相同的 parent 中引用多个元素，这将是非常乏味的，你需要一遍又一遍地写 parent。例如：
```css
section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text-decoration: underline;
}
```
而如果用 CSS 预处理器，就可以少些很多单词，而且父子节点关系一目了然。我们这里提到的三个 CSS 框架都是允许嵌套语法：
```css
section {
  margin: 10px;
  
  nav {
    height: 25px;
  
    a {
      color: #0982C1;
  
      &amp;:hover {
        text-decoration: underline;
      }
    }
  }
}
```
最终生成的 CSS 结果是：
```css
section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text-decoration: underline;
}
```
非常之方便！

# Mixins (混入)

Mixins 有点像是函数或者是宏，当你某段 CSS 经常需要在多个元素中使用时，你可以为这些共用的 CSS 定义一个 Mixin，然后你只需要在需要引用这些 CSS 地方调用该 Mixin 即可。

## Sass的混入语法
```css
/* Sass mixin error with (optional) argument $borderWidth which defaults to 2px if not specified */
@mixin error($borderWidth: 2px) {
  border: $borderWidth solid #F00;
  color: #F00;
}
  
.generic-error {
  padding: 20px;
  margin: 4px;
  @ include error(); /* Applies styles from mixin error */
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  @ include error(5px); /* Applies styles from mixin error with argument $borderWidth equal to 5px*/
}
```

## Less的混入语法
```css
/* LESS mixin error with (optional) argument @borderWidth which defaults to 2px if not specified */
.error(@borderWidth: 2px) {
  border: @borderWidth solid #F00;
  color: #F00;
}
  
.generic-error {
  padding: 20px;
  margin: 4px;
  .error(); /* Applies styles from mixin error */
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  .error(5px); /* Applies styles from mixin error with argument @borderWidth equal to 5px */
}
```

## Stylus的混入语法
```css
/* Stylus mixin error with (optional) argument borderWidth which defaults to 2px if not specified */
error(borderWidth= 2px) {
  border: borderWidth solid #F00;
  color: #F00;
}
  
.generic-error {
  padding: 20px;
  margin: 4px;
  error(); /* Applies styles from mixin error */
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  error(5px); /* Applies styles from mixin error with argument borderWidth equal to 5px */
}
```
最终它们都将编译成如下的 CSS 样式：
```css
.generic-error {
  padding: 20px;
  margin: 4px;
  border: 2px solid #f00;
  color: #f00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  border: 5px solid #f00;
  color: #f00;
}
```
# 继承

当我们需要为多个元素定义相同样式的时候，我们可以考虑使用继承的做法。例如我们经常需要：
```css
p,
ul,
ol {
  /* styles here */
}
```
## Sass和Stylus的继承
```css
.block {
  margin: 10px 5px;
  padding: 2px;
}
  
p {
  @extend .block; /* Inherit styles from '.block' */
  border: 1px solid #EEE;
}
ul, ol {
  @extend .block; /* Inherit styles from '.block' */
  color: #333;
  text-transform: uppercase;
}
```
在这里首先定义 .block 块，然后让 p 、ul 和 ol 元素继承 .block ，最终生成的 CSS 如下：
```css
.block, p, ul, ol {
  margin: 10px 5px;
  padding: 2px;
}
p {
  border: 1px solid #EEE;
}
ul, ol {
  color: #333;
  text-transform: uppercase;
}
```
## Less的继承
```css
.block {
  margin: 10px 5px;
  padding: 2px;
}
  
p {
  .block; /* Inherit styles from '.block' */
  border: 1px solid #EEE;
}
ul, ol {
  .block; /* Inherit styles from '.block' */
  color: #333;
  text-transform: uppercase;
}
```
生成的 CSS 如下：
```css
.block {
  margin: 10px 5px;
  padding: 2px;
}
p {
  margin: 10px 5px;
  padding: 2px;
  border: 1px solid #EEE;
}
ul,
ol {
  margin: 10px 5px;
  padding: 2px;
  color: #333;
  text-transform: uppercase;
}
```
你所看到的上面的代码中，.block 的样式将会被插入到相应的你想要继承的选择器中，但需要注意的是优先级的问题。

# 导入 (Import)
导入使用的后缀不要用css，用以上我们提到的三种对应格式。不然和css的导入相同，会增加http请求次数。

reset.xxx

```css
body {
  background: #EEE;
}
```
main.xxx
```css
@ import "reset";
  
p {
  background: #0982C1;
}
```
最终的main.css：
```css
body {
  background: #EEE;
}
p {
  background: #0982C1;
}
```
# 颜色函数

CSS 预处理器一般都会内置一些颜色处理函数用来对颜色值进行处理，例如加亮、变暗、颜色梯度等。
## Sass
```css
lighten($color, 10%); /* returns a color 10% lighter than $color */
darken($color, 10%);  /* returns a color 10% darker than $color */
  
saturate($color, 10%);   /* returns a color 10% more saturated than $color */
desaturate($color, 10%); /* returns a color 10% less saturated than $color */
  
grayscale($color);  /* returns grayscale of $color */
complement($color); /* returns complement color of $color */
invert($color);     /* returns inverted color of $color */
  
mix($color1, $color2, 50%); /* mix $color1 with $color2 with a weight of 50% */

```
上面只是简单列了 Sass 的一些基本颜色处理函数，完整的列表请看 [Sass Documentation](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html)

下面是一个具体的例子：
```css
$color: #0982C1;
  
h1 {
  background: $color;
  border: 3px solid darken($color, 50%);
}
```
## Less
```css
lighten(@color, 10%); /* returns a color 10% lighter than @color */
darken(@color, 10%);  /* returns a color 10% darker than @color */
  
saturate(@color, 10%);   /* returns a color 10% more saturated than @color */
desaturate(@color, 10%); /* returns a color 10% less saturated than @color */
  
spin(@color, 10);  /* returns a color with a 10 degree larger in hue than @color */
spin(@color, -10); /* returns a color with a 10 degree smaller hue than @color */
  
mix(@color1, @color2); /* return a mix of @color1 and @color2 */
```
LESS 完整的颜色函数列表请看 [LESS Documentation](http://lesscss.org/#-color-functions)

LESS 使用颜色函数的例子：
```css
@color: #0982C1;
  
h1 {
  background: @color;
  border: 3px solid darken(@color, 50%);
}
```
## Stylus
```css
lighten(color, 10%); /* returns a color 10% lighter than 'color' */
darken(color, 10%);  /* returns a color 10% darker than 'color' */
  
saturate(color, 10%);   /* returns a color 10% more saturated than 'color' */
desaturate(color, 10%); /* returns a color 10% less saturated than 'color' */
```
完整的颜色函数列表请阅读 [Stylus Documentation](http://learnboost.github.com/stylus/docs/bifs.html)

实例：
```css
color = #0982C1
  
h1
  background color
  border 3px solid darken(color, 50%)
```

# 运算符

你可以直接在 CSS 预处理器中进行样式的计算，例如：
```css
body {
  margin: (14px/2);
  top: 50px + 100px;
  right: 100px - 50px;
  left: 10 * 10;
}
```
# 一些跟具体浏览器相关的处理

这是宣传使用预处理的原因之一，并且是一个很好的理由，这样可以节省的大量的时间和汗水。创建一个mixin来处理不同浏览器的CSS写法是很简单的，节省了大量的重复工作和痛苦的代码编辑。

## Sass

```css
@mixin border-radius($values) {
  -webkit-border-radius: $values;
     -moz-border-radius: $values;
          border-radius: $values;
}
  
div {
  @ include border-radius(10px);
}
```
## Less

```css
.border-radius(@values) {
  -webkit-border-radius: @values;
     -moz-border-radius: @values;
          border-radius: @values;
}
  
div {
  .border-radius(10px);
}
```
## Stylus

```css
border-radius(values) {
  -webkit-border-radius: values;
     -moz-border-radius: values;
          border-radius: values;
}
  
div {
  border-radius(10px);
}
```
编译结果：

```css
div {
  -webkit-border-radius: 10px;
     -moz-border-radius: 10px;
          border-radius: 10px;
}
```
# 3D文本

要生成具有 3D 效果的文本可以使用 text-shadows ，唯一的问题就是当要修改颜色的时候就非常的麻烦，而通过 mixin 和颜色函数可以很轻松的实现：

##Sass

```css
@mixin text3d($color) {
  color: $color;
  text-shadow: 1px 1px 0px darken($color, 5%),
               2px 2px 0px darken($color, 10%),
               3px 3px 0px darken($color, 15%),
               4px 4px 0px darken($color, 20%),
               4px 4px 2px #000;
}
  
h1 {
  font-size: 32pt;
  @ include text3d(#0982c1);
}
```
## Less

```css
.text3d(@color) {
  color: @color;
  text-shadow: 1px 1px 0px darken(@color, 5%),
               2px 2px 0px darken(@color, 10%),
               3px 3px 0px darken(@color, 15%),
               4px 4px 0px darken(@color, 20%),
               4px 4px 2px #000;
}
  
span {
  font-size: 32pt;
  .text3d(#0982c1);
}
```
## Stylus

```css
text3d(color)
  color: csscolor
  text-shadow: 1px 1px 0px darken(color, 5%), 2px 2px 0px darken(color, 10%), 3px 3px 0px darken(color, 15%), 4px 4px 0px darken(color, 20%), 4px 4px 2px #000
span
  font-size: 32pt
  text3d(#0982c1)
```
生成的 CSS

```css
span {
  font-size: 32pt;
  color: #0982c1;
  text-shadow: 1px 1px 0px #097bb7,
               2px 2px 0px #0875ae,
               3px 3px 0px #086fa4,
               4px 4px 0px #07689a,
               4px 4px 2px #000;
}
```
# 列 (Columns)

使用数值操作和变量可以很方便的实现适应屏幕大小的布局处理。

## Sass

```css
$siteWidth: 1024px;
$gutterWidth: 20px;
$sidebarWidth: 300px;
  
body {
  margin: 0 auto;
  width: $siteWidth;
}
.content {
  float: left;
  width: $siteWidth - ($sidebarWidth+$gutterWidth);
}
.sidebar {
  float: left;
  margin-left: $gutterWidth;
  width: $sidebarWidth;
}
```
## Less

```css
@siteWidth: 1024px;
@gutterWidth: 20px;
@sidebarWidth: 300px;
  
body {
  margin: 0 auto;
  width: @siteWidth;
}
.content {
  float: left;
  width: @siteWidth - (@sidebarWidth+@gutterWidth);
}
.sidebar {
  float: left;
  margin-left: @gutterWidth;
  width: @sidebarWidth;
}
```
Stylus

```css
siteWidth = 1024px;
gutterWidth = 20px;
sidebarWidth = 300px;
  
body {
  margin: 0 auto;
  width: siteWidth;
}
.content {
  float: left;
  width: siteWidth - (sidebarWidth+gutterWidth);
}
.sidebar {
  float: left;
  margin-left: gutterWidth;
  width: sidebarWidth;
}
```
实际效果

```css
body {
  margin: 0 auto;
  width: 1024px;
}
.content {
  float: left;
  width: 704px;
}
.sidebar {
  float: left;
  margin-left: 20px;
  width: 300px;
}
```
# 错误报告

如果你经常 CSS 你会发现很难找到 CSS 中错误的地方，这也是预处理框架的好处，它会报告错误，你可以从[这篇](http://tjholowaychuk.com/post/5002088731/stylus-vs-sass-vs-less-error-reporting)文章中学习如何让 CSS 框架错误报告。

# 注释

以上三种框架都支持形如 /* */ 的多行注释以及 // 的单行注释。其中//注释会在编译时被删除。