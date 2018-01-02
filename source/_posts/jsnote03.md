---
title: javascript代码笔记03
categories: 前端学习笔记
tags: [js代码段,javascript]
comment: true
date: 2015-12-18 20:26:07
---
整理的一些javascript代码段，使用原生的js编写

<!-- more -->

# js数据类型转换

constructor 属性返回所有 JavaScript 变量的构造函数。

```javascript
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```

你可以使用 constructor 属性来查看是对象是否为数组 (包含字符串 "Array"):
```javascript
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}
```

## 数字转换为字符串

- toExponential() 把对象的值转换为指数计数法。
- toFixed() 把数字转换为字符串，结果的小数点后有指定位数的数字。
- toPrecision() 把数字格式化为指定的长度。

## 一元运算符 +

Operator + 可用于将变量转换为数字：
```javascript
var y = "5";      // y 是一个字符串
var x = + y;      // x 是一个数字
```

如果变量不能转换，它仍然会是一个数字，但值为 NaN (不是一个数字):
实例
```javascript
var y = "John";   // y 是一个字符串
var x = + y;      // x 是一个数字 (NaN)
```

# 正则表达式

- 正则表达式是由一个字符序列形成的搜索模式。
- 当你在文本中搜索数据时，你可以用搜索模式来描述你要查询的内容。
- 正则表达式可以是一个简单的字符，或一个更复杂的模式。
- 正则表达式可用于所有文本搜索和文本替换的操作。

语法格式：`var patt = /w3cschool/i`

## 使用字符串方法
在 JavaScript 中，正则表达式通常用于两个字符串方法 : search() 和 replace()。

**search() 方法** 用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回字符串的起始位置。

**replace() 方法**用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的字符串。

**search() 方法使用正则表达式**
```javascript
使用正则表达式搜索 "w3cschool" 字符串，且不区分大小写：
var str = "Visit w3cschool";
var n = str.search(/w3cschool/i);
```
**replace() 方法使用正则表达式**
```javascript
使用正则表达式且不区分大小写将字符串中的 Microsoft 替换为 w3cschool :
var str = "Visit Microsoft!";
var res = str.replace(/microsoft/i, "w3cschool");
```

## 使用 RegExp 对象
在 JavaScript 中，RegExp 对象是一个预定义了属性和方法的正则表达式对象。

# js错误

在下面的例子中，我们故意在 try 块的代码中写了一个错字。catch 块会捕捉到 try 块中的错误，并执行代码来处理它。
```javascript
var txt="";
function message()
{
    try {
        adddlert("Welcome guest!");
    } catch(err) {
        txt="本页有一个错误。\n\n";
        txt+="错误描述：" + err.message + "\n\n";
        txt+="点击确定继续。\n\n";
        alert(txt);
    }
}
```
本例检测输入变量的值。如果值是错误的，会抛出一个异常（错误）。catch 会捕捉到这个错误，并显示一段自定义的错误消息：
```javascript
function myFunction()
{
  try
  {
    var x=document.getElementById("demo").value;
    if(x=="")    throw "值为空";
    if(isNaN(x)) throw "不是数字";
    if(x > 10) throw "太大";
    if(x < 5) throw "太小";
  }
  catch(err)
  {
    var y=document.getElementById("mess");
    y.innerHTML="错误：" + err + "。";
  }
}
```
