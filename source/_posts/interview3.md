---
title: 前端 JS 高级面试
comment: true
date: 2018-08-14 17:12:35
tags: 面试
categories: 前端学习笔记
---

前端 JS 高级知识整理，以及面试要点。

<!-- more -->

- 基础知识
  - ES6 常用语法
  - 原型高级应用
  - 异步全面讲解
- 框架原理
  - 虚拟 DOM
  - MVVM Vue
  - 组件化 React
- 混合开发
  - hybrid
  - hybrid vs H5
  - 前端客户端通讯
- 热爱编程
  - 读书 博客
  - 开源  

## ES6 常用语法

### 模块化的使用和编译环境
[webpack笔记](#)
[babel使用](https://github.com/amenzai/code-snippet/blob/master/js/babel.md)
rollup.js 使用：
- 功能单一， webpack功能强大
- 参考设计原则和Linux设计思想
- 工具要尽量功能单一，可集成，可扩展
- wangEdtor 用的 gulp + rollup

### Class 与 JS 构造函数的区别

- Class 在语法上更加铁盒面向对象的写法
- Class 实现继承更加易读、易理解
- 更易于写 java 等后端语言的使用
- 本质还是语法糖，使用 prototype

[参考1-构造函数和集成ES5](http://amenzai.me/javascript-book/chapter3/section3.3.html)
[参考2-class 和 extends](https://github.com/amenzai/code-snippet/blob/master/js/ES6.md#%E6%80%BB%E6%98%AF%E7%94%A8class%E5%8F%96%E4%BB%A3%E9%9C%80%E8%A6%81prototype%E7%9A%84%E6%93%8D%E4%BD%9C%E5%9B%A0%E4%B8%BAclass%E7%9A%84%E5%86%99%E6%B3%95%E6%9B%B4%E7%AE%80%E6%B4%81%E6%9B%B4%E6%98%93%E4%BA%8E%E7%90%86%E8%A7%A3)

### Promise 的用法

- 定义方法，return promise 实例
- new Promise 时要传入函数，函数有 resolve reject 两个参数
- 执行成功 resolve() 失败 reject()
- then() 监听成功结果 catch() 监听失败结果

[参考](https://github.com/amenzai/code-snippet/blob/master/js/ES6.md#promise)

### ES6 其他常用功能
[参考](https://github.com/amenzai/code-snippet/blob/master/js/ES6.md)

## 原型高级应用

### 原型如何实际应用
jquery 和 zepto 的简单使用
他俩如何使用原型

**zepto 如何使用原型**

```js
var zepto = {}
zepto.init = function(selector) {
  // resoure code 略 only write prototype
  var slice = Array.prototype.slice
  var dom = slice.call(document.querySelectorAll(selector))
  return zepto.Z(dom, selector)
}

// use
var $ = function(selector) {
  return zepto.init(selector)
}

// 构造函数
function Z(dom, selector) {
  var i, len = dom ? dom.length : 0
  for(i = 0; i < len; i++) {
    this[i] = dom[i]
  }
  this.length = len
  this.selector = selector || ''
}
zepto.Z = function(dom, selector) {
  return new Z(dom, selector)
}

$.fn = {
  constructor: zepto.Z,
  css: function(key, value) {

  },
  html: function(value) {

  }
}
zepto.Z.prototype = Z.prototype = $.fn
```

**jQuery 如何使用原型**
```js
(function(window) {

  var jQuery = function(selector) {
    return new jQuery.fn.init(selector)
  }

  jQuery.fn = {
    constructor: jQuery,
    css: function(key, value) {
      alert('css')
    },
    html: function(value) {
      return 'html'
    }
  }

  var init = jQuery.fn.init = function(selector) {
    var slice = Array.prototype.slice
    var dom = slice.call(document.querySelectorAll(selector))

    var i, len = dom ? dom.length : 0
    for (i = 0; i < len; i++) {
      this[i] = dom[i]
    }
    this.length = len
    this.selector = selector || ''
  }

  init.prototype = jQuery.fn

  window.$ = jQuery

})(window)
```

- 描述一下 jQuery 如何使用原型
- 。。。。zepto。。。。。。
- 再结合自己的项目经验，说一下自己开发的例子


### 原型如何满足扩展

$.fn.myMethod = function() {}

- 说一下 jQuery 和 zepto 的插件机制
- 结合自己开发经验，做过的基于原型的插件

## 异步全面讲解

### 什么是单线程，和异步有何关系？什么是 event-loop
单线程：只有一个线程，同时只能做一件事（for 循环多次、alert），两段JS不能同时执行
原因：避免DOM渲染的冲突
解决方案：异步

- 事件轮询，JS实现异步的具体解决方案
- 同步代码直接执行
- 异步函数先放在异步队列
- 待同步函数执行完毕，轮询执行异步队列的函数

`什么是异步队列？何时被放入异步队列？轮询的过程`

### 目前 JS 解决异步的方案有哪些
- jQuery Deferred
- Promise
- Async / Await
- Generator
  - 原理比较复杂
  - 不是异步的直接替代方式
  - 有更好更简洁的解决方案 async / await
  - koa 也早已“弃暗投明”

### 如果只用 jQuery 如何解决异步
jquery1.5的变化：
- 无法改变JS异步和单线程的本质
- 只能从写法上杜绝 callback 这种形式
- 他是一种语法糖形式，但是解耦了代码
- 很好的体现：开放封闭原则

$.ajax().done().fail().done()
$.ajax().then(function(){}, function(){}).then()

jquery-deferred: 对扩展开放，对修改关闭
dtdAPI分为两类，用意不同：
- 第一类：dtd.resolve(), dtd.reject
- 第二类：dtd.then dtd.done dtd.fail
- 这两类应该分开，否则后果很严重

```js
function waitHandle() {
    // 定义
    var dtd = $.Deferred()
    var wait = function(dtd) {
        var task = function() {
          console.log('执行完成')
            // 成功
          dtd.resolve()
            // 失败
            // dtd.reject()
        }
        setTimeout(task, 1000)
          // wait 返回
        return dtd.promise()
      }
      // 最终返回
    return wait(dtd)
  }

  // 使用（B 员工）
  var w = waitHandle() // promise 对象
  $.when(w).then(function() {
    console.log('ok 1')
  }, function() {
    console.log('err 1')
  })
```

### Promise 的标准
- 状态变化
  - 三种状态：pending fulfilled rejected
  - 初始状态：pending
  - pending 变为 fulfilled，或者 pending 变为 rejected
  - 状态变化不可逆
- then
  - Promise 实例必须实现 then 这个方法
  - then() 必须可以接受两个函数作为参数
  - then() 返回的必须是一个 Promise 实例

问题：
- 基本语法
- 如何异常捕获（Error 和 reject 都要考虑）
- 多个串联-链式执行的好处
- Promise.all 和 Promise.race
- Promise 标准 - 状态变化，then 函数

### async / await 的使用
- then 只是将 callback 拆分了
- async/await 是最直接的同步写法
- 使用 await，函数必须用 async 标识
- await 后面跟的是一个 Promise 实例
- 需要 babel-polyfill

问题：
- 基本语法
- 使用了 Promise，并没有和 Promise 冲突
- 完全是同步的写法，再也没有回调函数
- 但是：改变不了JS单线程、异步的本质

## 虚拟DOM

### 什么是Vdom，为何要用Vdom

设计一个需求场景 -》用jquery实现 -》遇到的问题

浏览器渲染 DOM 是很慢的，运行 JS 快成一匹马

```js
// 点击按钮，改变表格数据
// jquery demo

// html
// <div id="container"></div>
// <button id="btn-change">change</button>

// js
var data = [{
    name: '张三',
    age: '20',
    address: '北京'
  },
  {
    name: '李四',
    age: '21',
    address: '上海'
  },
  {
    name: '王五',
    age: '22',
    address: '广州'
  }
]

// 渲染函数
function render(data) {
  var $container = $('#container')

  // 清空容器，重要！！！
  $container.html('')

  // 拼接 table
  var $table = $('<table>')

  $table.append($('<tr><td>name</td><td>age</td><td>address</td>/tr>'))
  data.forEach(function (item) {
    $table.append($('<tr><td>' + item.name + '</td><td>' + item.age + '</td><td>' + item.address +
      '</td>/tr>'))
  })

  // 渲染到页面
  $container.append($table)
}

$('#btn-change').click(function () {
  data[1].age = 30
  data[2].address = '深圳'
  // re-render  再次渲染
  render(data)
})

// 页面加载完立刻执行（初次渲染）
render(data)

// 存在的问题就是，没有改变数据的地方，也会重新渲染
```

遇到的问题：
- DOM 操作是“昂贵的”，JS 运行效率高
- 尽量减少 DOM 操作，而不是“推倒重来”
- 项目越复杂 ，影响就越严重
- vdom即可解决问题

vdom：
- 用 JS 模拟 DOM 结构
- DOM 变化的对比，放在 JS 层来做（图灵完备语言）
- 提高重绘性能

### Vdom如何使用，核心函数有哪些
snabbdom用法：

```js
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom.js"></script>
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-class.js"></script>
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-props.js"></script>
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-style.js"></script>
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-eventlisteners.js"></script>
// <script src="https://cdn.bootcss.com/snabbdom/0.7.1/h.js"></script>
var snabbdom = window.snabbdom

// 定义 patch
var patch = snabbdom.init([
  snabbdom_class,
  snabbdom_props,
  snabbdom_style,
  snabbdom_eventlisteners
])

// 定义 h
var h = snabbdom.h

var container = document.getElementById('container')

// 生成 vnode
var vnode = h('ul#list', {}, [
  h('li.item', {}, 'Item 1'),
  h('li.item', {}, 'Item 2')
])
patch(container, vnode)

document.getElementById('btn-change').addEventListener('click', function () {
  // 生成 newVnode
  var newVnode = h('ul#list', {}, [
    h('li.item', {}, 'Item 1'),
    h('li.item', {}, 'Item B'),
    h('li.item', {}, 'Item 3')
  ])
  patch(vnode, newVnode) // 找出差异，渲染差异
})
```

```js
// jquery例子改造
var snabbdom = window.snabbdom
// 定义关键函数 patch
var patch = snabbdom.init([
  snabbdom_class,
  snabbdom_props,
  snabbdom_style,
  snabbdom_eventlisteners
])

// 定义关键函数 h
var h = snabbdom.h

// 原始数据
var data = [{
    name: '张三',
    age: '20',
    address: '北京'
  },
  {
    name: '李四',
    age: '21',
    address: '上海'
  },
  {
    name: '王五',
    age: '22',
    address: '广州'
  }
]
// 把表头也放在 data 中
data.unshift({
  name: '姓名',
  age: '年龄',
  address: '地址'
})

var container = document.getElementById('container')

// 渲染函数
var vnode

function render(data) {
  var newVnode = h('table', {}, data.map(function (item) {
    var tds = []
    var i
    for (i in item) {
      if (item.hasOwnProperty(i)) {
        tds.push(h('td', {}, item[i] + ''))
      }
    }
    return h('tr', {}, tds)
  }))

  if (vnode) {
    // re-render
    patch(vnode, newVnode)
  } else {
    // 初次渲染
    patch(container, newVnode)
  }

  // 存储当前的 vnode 结果
  vnode = newVnode
}

// 初次渲染
render(data)


var btnChange = document.getElementById('btn-change')
btnChange.addEventListener('click', function () {
  data[1].age = 30
  data[2].address = '深圳'
  // re-render
  render(data)
})
```

核心API：
- h(‘<标签名>’, {…属性…}, […子元素…])
- h(‘<标签名>’, {…属性…}, ‘….’)
- patch(container, vnode)
- patch(vnode, newVnode) 


### 了解diff算法吗
- 什么是diff算法 
  - linux diff 命令
  - git diff （对比两个文件之间差异）
- 去繁就简
  - diff 算法非常复杂，实现难度很大，源码量很大
  - 去繁就简，讲明白核心流程，不关心细节
  - 面试官也大部分都不清楚细节，但是很关心核心流程
  - 去繁就简之后，依然具有很大挑战性，并不简单
- vdom 为何用 diff 算法
  - DOM 操作是“昂贵”的，因此尽量减少 DOM 操作
  - 找出本次 DOM 必须更新的节点来更新，其他的不更新
  - 这个“找出”的过程，就需要 diff 算法
- diff 算法的实现流程
  - patch(container, vnode)
  - patch(vnode, newVnode)
- 核心逻辑：createElement 和 updateChildren

```js
// diff 算法实现
// code demo
function createElement(vnode) {
  var tag = vnode.tag // 'ul'
  var attrs = vnode.attrs || {}
  var children = vnode.children || []
  if (!tag) {
    return null
  }

  // 创建真实的 DOM 元素
  var elem = document.createElement(tag)
  // 属性
  var attrName
  for (attrName in attrs) {
    if (attrs.hasOwnProperty(attrName)) {
      // 给 elem 添加属性
      elem.setAttribute(attrName, attrs[attrName])
    }
  }
  // 子元素
  children.forEach(function (childVnode) {
    // 给 elem 添加子元素
    elem.appendChild(createElement(childVnode)) // 递归
  })

  // 返回真实的 DOM 元素
  return elem
}

// vnode newVnode compare
function updateChildren(vnode, newVnode) {
  var children = vnode.children || []
  var newChildren = newVnode.children || []

  children.forEach(function (childVnode, index) {
    var newChildVnode = newChildren[index]
    if (childVnode.tag === newChildVnode.tag) {
      // 深层次对比，递归
      updateChildren(childVnode, newChildVnode)
    } else {
      // 替换
      replaceNode(childVnode, newChildVnode)
    }
  })
}

function replaceNode(vnode, newVnode) {
  var elem = vnode.elem // 真实的 DOM 节点
  var newElem = createElement(newVnode)

  // 替换
}
```

answer:
- 知道什么是 diff 算法，是 linux 的基础命令
- vdom 中应用 diff 算法是为了找出需要更新的节点
- vdom 实现过程，createElement 和 updateChildren
- 与核心函数 patch 的关系

## MVVM（Vue）

- 如何理解 MVVM
- 如何实现 MVVM
- 是否解读过 vue 的源码（理解能力 学习能力 自学能力）

## 说一下使用 jQuery 和使用框架的区别

todolist

```js
// jquery
// html

// <div>
//   <input type="text" name="" id="txt-title">
//   <button id="btn-submit">submit</button>
// </div>
// <div>
//   <ul id="ul-list"></ul>
// </div>
var $txtTitle = $('#txt-title')
var $btnSubmit = $('#btn-submit')
var $ulList = $('#ul-list')
$btnSubmit.click(function () {
  var title = $txtTitle.val()
  if (!title) {
    return
  }
  var $li = $('<li>' + title + '</li>')
  $ulList.append($li)
  $txtTitle.val('')
})



// vue

// <div id="app">
//   <div>
//     <input v-model="title">
//     <button v-on:click="add">submit</button>
//   </div>
//   <div>
//     <ul>
//       <li v-for="item in list">{{item}}</li>
//     </ul>
//   </div>
// </div>

// data 独立
var data = {
  title: '',
  list: []
}
// 初始化 Vue 实例
var vm = new Vue({
  el: '#app',
  data: data,
  methods: {
    add: function () {
      this.list.push(this.title)
      this.title = ''
    }
  }
})
/*
    
with(this){  // this 就是 vm
    return _c(
        'div',
        {
            attrs:{"id":"app"}
        },
        [
            _c(
                'div',
                [
                    _c(
                        'input',
                        {
                            directives:[
                                {
                                    name:"model",
                                    rawName:"v-model",
                                    value:(title),
                                    expression:"title"
                                }
                            ],
                            domProps:{
                                "value":(title)
                            },
                            on:{
                                "input":function($event){
                                    if($event.target.composing)return;
                                    title=$event.target.value
                                }
                            }
                        }
                    ),
                    _v(" "),
                    _c(
                        'button',
                        {
                            on:{
                                "click":add
                            }
                        },
                        [_v("submit")]
                    )
                ]
            ),
            _v(" "),
            _c('div',
                [
                    _c(
                        'ul',
                        _l((list),function(item){return _c('li',[_v(_s(item))])})
                    )
                ]
            )
        ]
    )
}

*/
```

区别：
- 数据和视图的分离，解耦（开放封闭原则）
- 以数据驱动视图，只关心数据变化，DOM 操作被封装

### 说一下对 MVVM 的理解
MVC：
- M：Model 数据
- V：View 视图、界面
- C：Controller 控制器、逻辑处理

MVVM：
- Model：模型、数据
- View：视图、模板（视图和模型是分离的）
- ViewModel：连接 Model 和 View

关于 ViewModel
- MVVM 不算是一种创新
- 但其中的 ViewModel 确实一种创新
- 真正结合前端场景应用的创建

解答：
- MVVM：Model View ViewModel
- 三者之间的联系、以及如何对应到各段代码
- ViewModel 的理解，联系 View 和 Model

MVVM三要素：
- 响应式：vue 如何监听到 data 的每个属性变化
- 模板引擎：Vue 的模板如何被解析，指令如何处理
- 渲染：vue 的模板如何被渲染成html？以及渲染过程
### vue 中如何实现响应式
- 什么是响应式
  - 修改 data 属性之后，vue 立刻监听到
  - data 属性被代理到 vm 中
- Object.defineProperty
- 模拟

```js
// var obj = {
//     name: 'zhangsan',
//     age: 25
// }
// console.log(obj)

// var obj = {}
// var _name = 'shangsan'
// Object.defineProperty(obj, 'name', {
//     get: function () {
//         console.log('get', _name) // 监听
//         return _name
//     },
//     set: function (newVal) {
//         console.log('set', newVal)  // 监听
//         _name = newVal
//     }
// })


// var vm = new Vue({
//     el: '#app',
//     data: {
//         name: 'zhangsan',
//         age: 20
//     }
// })

var vm = {}
var data = {
  name: 'zhangsan',
  age: 20
}

var key, value
for (key in data) {
  (function (key) { // 保证 key 的独立作用域
    Object.defineProperty(vm, key, {
      get: function () {
        console.log('get', data[key]) // 监听
        return data[key]
      },
      set: function (newVal) {
        console.log('set', newVal) // 监听
        data[key] = newVal
      }
    })
  })(key)
}
```
答案：
- 关键是理解 Object.defineProperty
- 将 data 的属性代理到 vm 上
### vue 中如何解析模板
- 模板是什么
  - 本质：字符串
  - 有逻辑，如 v-if v-for 等
  - 与 html 格式很想，但有很大区别
  - 最终还要转换为 html 来显示
  - 分割线
  - 模板最终必须转换成 JS 代码，因为
  - 有逻辑（v-if v-for），必须用 JS 才能实现
  - 转换为 html 渲染页面，必须用 JS 才能实现
  - 因此，模板最终要转换成一个 JS 函数（render 函数）
- render 函数
  - 模板中所有信息都包含在了 render 函数中
  - this 即 vm
  - price 即 this.price 即 vm.price，即 data 中的 price
  - _c 即 this._c 即 vm._c
  - 从哪里可以看到 render 函数？(源码查找code.render 打印出来)
  - 复杂一点的例子，render 函数是什么样子的？（参考上面的 todo-list）
  - vm._c 是什么？（创建元素）
- render 函数与 vdom
  - vm._c 其实就相当于 snabbdom 中的 h 函数
  - render 函数执行之后，返回的是 vnode
  - updateComponent 中实现了 vdom 的 patch
  - 页面首次渲染执行 updateComponent
  - data 中每次修改属性，执行 updateComponent

根据 todo-list demo 的 render 函数：
- v-model 是怎么实现的？
- v-on:click 是怎么实现的？
- v-for 是怎么实现的？

render函数解决了模板中“逻辑”（v-for v-if）的问题
还剩下模板生成 html 的问题
另外，vm._c 是什么？render 函数返回了什么？

```js
// html
// <div id="app">
//   <p>{{price}}</p>
// </div>

// js
var vm = new Vue({
  el: '#app',
  data: {
    price: 100
  }
})

// 以下是手写的 ppt 中的 render 函数
function render() {
  with(this) { // this 就是 vm
    return _c(
      'div', {
        attrs: {
          'id': 'app'
        }
      }, [
        _c('p', [_v(_s(price))])
      ]
    )
  }
}

function render1() {
  return vm._c(
    'div', {
      attrs: {
        'id': 'app'
      }
    }, [
      vm._c('p', [vm._v(vm._s(vm.price))])
    ]
  )
}
```

### vue 的整个实现流程

- 模板：字符串，有逻辑，嵌入 JS 变量……
- 模板必须转换为 JS 代码（有逻辑、渲染 html、JS 变量）
- render 函数是什么样子的
- render 函数执行是返回 vnode
- updateComponent

- 第一步：解析模板成 render 函数
  - with 的用法
  - 模板中的所有信息都被 render 函数包含
  - 模板中用到的 data 中的属性，都变成了 JS 变量
  - 模板中的 v-model  v-for  v-on 都变成了 JS 逻辑
  - render 函数返回 vnode 
- 第二步：响应式开始监听
  - Object.defineProperty
  - 将 data 的属性代理到 vm 上
- 第三步：首次渲染，显示页面，且绑定依赖
  - 初次渲染，执行 updateComponent，执行 vm._render()
  - 执行 render 函数，会访问到 vm.list vm.title
  - 会被响应式的 get 方法监听到（后面详细讲）
  - 执行 updateComponent ，会走到 vdom 的 patch 方法
  - patch 将 vnode 渲染成 DOM ，初次渲染完成
- 第四步：data 属性变化，触发 rerender
  - 修改属性，被响应式的 set 监听到
  - set 中执行 updateComponent
  - updateComponent 重新执行 vm._render()
  - 生成的 vnode 和 prevVnode ，通过 patch 进行对比
  - 渲染到 html 中

## 组件化 React

### 说一下对组件化的理解

- 组件化的封装
  - 视图
  - 数据
  - 变化的逻辑（数据驱动视图变化）
- 组建的复用
  - props 传递
  - 复用

### JSX 本质是什么
- JSX 语法
  - html 形式
  - 引入 JS 变量和表达式
  - if...else...
  - 循环
  - style 和 className
  - 事件
- JSX 解析成 JS
  - JSX 其实是语法糖
  - 开发环境会将 JSX 编译成 JS 代码
  - JSX 的写法大大降低了学习成本和编码工作量
  - 同时，JSX 也会增加 debug 成本
- 独立的标准
  - JSX 是 React 引入的，但不是 React 独有的
  - React 已经将它作为一个独立标准开放，其他项目也可用
  - React.createElement 是可以自定义修改的
  - 说明：本身功能已经完备；和其他标准兼容和扩展性没问题

```js
// jsx test
// devDependencies: babel-plugin-transform-react-jsx
// .babelrc
// {"plugins": ["transform-react-jsx"]}

// demo.js

/* @jsx h */
// 上面这段注释，用于更改 React.cerateElement()函数名称
// var profile = <div>
//   <img src="avatar.png" className="profile" />
//   <h3>{[user.firstName, user.lastName].join(' ')}</h3>
// </div>;

// class Input extends Component {
//   render() {
//     return (
//       <div>
//           <input value={this.state.title} onChange={this.changeHandle.bind(this)}/>
//           <button onClick={this.clickHandle.bind(this)}>submit</button>
//       </div>
//     );
//   }
// }

// 命令行输入：babel --plugins trnasform-react-jsx demo.jsx
```

### JSX 和 vdom 的关系
- 为何需要 vdom
  - vdom 是 React 初次推广开来的，结合 JSX
  - JSX 就是模板，最终要渲染成 html
  - 初次渲染 + 修改 state 后的 re-render
  - 正好符合 vdom 的应用场景
- React.createElement 和 h 都生成 vndoe
- 何时 patch ？
  - 初次渲染 - ReactDOM.render(<App/>, container)
  - 会触发 patch(container, vnode)
  - re-render - setState
  - 会触发 patch(vnode, newVnode)
- 自定义组件的解析
  - ‘div’ - 直接渲染 `<div>` 即可，vdom 可以做到
  - Input 和 List ，是自定义组件（class），vdom 默认不认识
  - 因此 Input 和 List 定义的时候必须声明 render 函数
  - 根据 props 初始化实例，然后执行实例的 render 函数
  - render 函数返回的还是 vnode对象

### 说一下 setState 的过程
- setState 的异步（为何要异步）
  - 可能会一次执行多次
  - 你无法规定、限制用户如何使用 setState
  - 没必要每次 setState 都重新渲染
  - 即便每次重新渲染，用户也看不到中间的效果
  - 只看到最后的结果即可
- vue 修改属性也是异步
- setState 的过程
  - 每个组件实例，都有 renderComponent 方法
  - 执行 renderComponent  会重新执行实例的 render
  - render 函数返回 newVnode ，然后拿到 preVnode 
  - 执行 patch(preVnode, newVnode)  

### 阐述自己对 React 和 vue 的认识
- 两者的本质区别
  - vue：本质是 MVVM 框架，有 MVC 发展而来
  - React：本质是前端组件化框架，由后端组件化发展而来
  - 但这并不妨碍他们两者都能实现相同的功能
- 模板的区别
  - vue - 使用模板（最初由 angular 提出）
  - React - 使用 JSX
  - 模板语法上，我更加倾向于 JSX
  - 模板分离上，我更加倾向于 vue
  - 模板应该和 JS 逻辑分离
  - 回顾“开放封闭原则”
- 组件化的区别
  - React 本身就是组件化，没有组件化就不是 React
  - vue 也支持组件化，不过是在 MVVM 上的扩展
  - 查阅 vue 组件化的文档，洋洋洒洒很多（侧面反映）
  - 对于组件化，我更加倾向于 React ，做的彻底而清晰
- 两者共同点
  - 都支持组件化
  - 都是数据驱动试图

问题解答：
- 国内使用，首推 vue 。文档更易读、易学、社区够大
- 如果团队水平较高，推荐使用 React 。组件化和 JSX

## hybrid

- 移动端占大部分流量，已经远远超过 PC
- 一线互联网公司都有自己的 App
- 这些 App 中有很大比例的前端代码（不要惊讶）
- 拿微信举例，你每天浏览微信的内容，多少是前端？

### hybrid 是什么，为何用 hybrid？
- hybrid 文字解释
  - hybrid 即“混合”，即前端和客户端的混合开发
  - 需前端开发人员和客户端开发人员配合完成
  - 某些环节也可能涉及到 server 端
  - PS：不要以为自己的前端就可以不理会客户端的知识
- 存在价值，为何会用 hybrid
  - 可以快速迭代更新【关键】（无需 app 审核，思考为何？）
  - 体验流畅（和 NA 的体验基本类似）
  - 减少开发和沟通成本，双端公用一套代码
- webview
  - 是 app 中的一个组件（ app 可以有 webview ，也可以没有）
  - 用于加载 h5 页面，即一个小型的浏览器内核
- file:// 协议
  - 其实在一开始接触 html 开发，就已经使用了 file 协议
  - 只不过你当时没有“协议”“标准”等这些概念
  - 再次强调“协议”“标准”的重要性！！！
  - file 协议：本地文件，快
  - http(s) 协议：网络加载，慢
- hybrid 实现流程
  - 以下是前言：
  - 不是所有场景都适合使用 hybrid：
  - 使用 NA ：体验要求极致，变化不频繁（如头条的首页）
  - 使用 hybrid ：体验要求高，变化频繁（如头条的新闻详情页）
  - 使用 h5 ：体验无要求，不常用（如举报、反馈等页面）
  - 以下是具体流程
  - 前端做好静态页面（html js css），将文件交给客户端
  - 客户端拿到前端静态页面，以文件形式存储在 app 中
  - 客户端在一个 webview 中
  - 使用 file 协议加载静态页面

### 介绍一下 hybrid 更新和上线的流程？
- 分版本，有版本号，如 201803211015
- 将静态文件压缩成 zip 包，上传到服务端
- 客户端每次启动，都去服务端检查版本号
- 如果服务端版本号大于客户端版本号，就去下载最新的 zip 包
- 下载完之后解压包，然后将现有文件覆盖

### hybrid 和 h5 的主要区别
- 优点
  - 体验更好，跟 NA 体验基本一致
  - 可快速迭代，无需 app 审核【关键】
- 缺点
  - 开发成本高。联调、测试、查 bug 都比较麻烦
  - 运维成本高。参考此前讲过的更新上线的流程
- 适用的场景
  - hybrid ：产品的稳定功能，体验要求高，迭代频繁
  - h5 ：单词次运营活动（如 xx 红包）或不常用功能

### 前端 JS 和客户端如何通讯？
[微信JSDK](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
![20180815105719](https://user-images.githubusercontent.com/20960902/44129348-558ea85a-a07a-11e8-9059-63df6addcc63.png)

- JS 和客户端通讯的基本形式
  - JS 访问客户端能力，传递参数和回调函数
  - 客户端通过回调函数返回内容
- schema 协议简介和使用
  - schema 协议 —— 前端和客户端通讯的约定
  - 使用iframe （类似JSONP）
  例如：weixin://dl/scan 扫一扫
- schema 使用的封装
- 内置上线  
  - 将以上封装的代码打包，叫做 invoke.js，内置到客户端
  - 客户端每次启动 webview ，都默认执行 invoke.js 
  - 本地加载，免去网络加载的时间，更快
  - 本地加载，没有网络请求，黑客看不到 schema 协议，更安全

```js
// schema 使用的封装

// html
// <button id="btn1">扫一扫</button>
// <button id="btn2">分享</button>

// <script type="text/javascript" src="./invoke.js"></script>

(function(window, undefined) {

  // 调用 schema 的封装
  function _invoke(action, data, callback) {
    // 拼装 schema 协议
    var schema = 'myapp://utils/' + action
    schema += schema.indexOf('?') === -1 ? '?' : '&'
      // 拼接参数
    for (var key in data) {
      if (data.hasOwnProperty(key)) {
        schema += key + '=' + data[key] + '&'
      }
    }
      // 处理 callback
    callbackName = action + Date.now()
    window[callbackName] = callback
    schema += 'callback=' + callbackName

    // 触发
    var iframe = document.createElement('iframe')
    iframe.style.display = 'none'
    iframe.src = schema // 重要！
    var body = document.body
    body.appendChild(iframe)
    setTimeout(function() {
      body.removeChild(iframe)
      iframe = null
    })
  }

  // 暴露到全局变量
  window.invoke = {
    share: function(data, callback) {
      _invoke('share', data, callback)
    },
    scan: function(data, callback) {
      _invoke('scan', data, callback)
    }
    login: function(data, callback) {
      _invoke('login', data, callback)
    }
  }

})(window)
```

## 你热爱编程吗

- 读书
  - 构建知识体系最好的方式
  - 自己买书
  - 看书有技巧（标记、有痕迹）
  - 看完书要总结（读书笔记、思维导图）
- 写博客
- 做开源