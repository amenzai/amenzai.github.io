---
title: js开发规范
categories: 前端学习笔记
tags: javascript
comment: true
date: 2017-11-01 20:26:07
---

如何编写规范且便于维护的js代码，请往下看-。-

<!-- more -->

## 命名规范

### 变量

- 命名方法：小驼峰式命名法。
- 命名规范：前缀应当是名词。(函数的名字前缀为动词，以此区分变量和函数)
- 命名建议：尽量在变量名字中体现所属类型，如:length、count等表示数字类型；而包含name、title表示为字符串类型。

示例：

```js
var maxCount = 10;
var tableTitle = 'LoginTable';
```

### 函数

- 命名方法：小驼峰式命名法。
- 命名规范：前缀应当为动词。
- 命名建议：可使用常见动词约定。

常见动词示例：

- can：判断是否可执行某个动作
- has：判断是否含有某个值
- is： 判断是否为某个值
- set：设置某个值
- get：获取某个值
- remove：移除某个值
- load：加载某些数据

代码示例：
```js
// 是否可阅读
function canRead() {
 return true;
}
 
// 获取名称
function getName() {
 return this.name;
}
```

### 常量

- 命名方法：名称全部大写
- 命名规范：使用大写字母和下划线来组合命名，下划线用以分割单词。

示例：
```js
var MAX_COUNT = 10;
var URL = 'http://www.baidu.com';
```

### 构造函数

- 命名方法：大驼峰式命名法，首字母大写。
- 命名规范：前缀为名称。

示例：

```js
function Student(name) {
 this.name = name;
}
 
var st = new Student('tom');
```

### 类的成员

- 公共属性和方法：跟变量和函数的命名一样。
- 私有属性和方法：前缀为_(下划线)，后面跟公共属性和方法一样的命名方式。

示例：

```js
function Student(name) {
  var _name = name; // 私有成员

  // 公共方法
  this.getName = function () {
  return _name;
 }
 
 // 公共方式
  this.setName = function (value) {
    _name = value;
  }
}
var st = new Student('tom');
st.setName('jerry');
console.log(st.getName()); // => jerry：输出_name私有变量的值
```

## 注释规范

### 单行注释

示例：

```js
// 调用了一个函数；1)单独在一行
setTitle();
 
var maxCount = 10; // 设置最大量；2)在代码后面注释
 
// setName(); // 3)注释代码
```

### 多行注释

```js
/*
* 代码执行到这里后会调用setTitle()函数
* setTitle()：设置title的值
*/
setTitle();
```

### 函数(方法)注释

```js
// sublime DocBlockr 插件可以自动生成

/**
 * [funcDemo description]
 * @param  {[type]} param1 [description]
 * @param  {[type]} param2 [description]
 * @return {[type]}        [description]
 */
function funcDemo (param1,param2) {
  return param1 + param2
}
```

## 框架开发

### 全局变量冲突

团队开发或者引入第三方JS文件时，有时会造成全局对象的名称冲突。

### 单全局变量
所创建的全局对象名称是独一无二的，并将所有的功能代码添加到这个全局对象上。调用自己所写的代码时，以这个全局对象为入口点。

例如：

* JQuery的全局对象：$和JQuery
* ExtJS的全局对象： Ext

### 命名空间

在项目规模日益壮大时，可采用命名空间方式对JS代码进行规范：即将代码按照功能进行分组，以组的形式附加到单全局对象上。
