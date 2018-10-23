---
title: 前端 JS 中级面试
comment: true
date: 2018-07-14 17:12:35
tags: 面试
categories: 前端学习笔记
---

前端面试进阶内容。

<!-- more -->


**面试环节：**

- 一面：基础知识 html css3 js
- 二面/三面（高级工程师）：基础延伸（根据简历上自己的项目）、技术原理 优点 缺点
- 三面/四面（业务负责人）：项目中担任的角色，起的作用，业务处理
- 终面（HR）：性格、沟通、潜能、规划

-----

**面试准备：**

- 职位描述分析
  - 通过了解招聘公司的岗位描述和要求，搞清楚需要了解那些技术点，然后查阅资料搞懂
  - 深入分析，弄清侧重的技术点
- 业务分析或实战模拟
  - 了解公司是哪个方向的业务，可以访问官网，查看源码了解技术栈
  - 比如：对于Jquery,了解下jQuery模板引擎、jQuery事件委托、事件代理、waifer延迟等
- 技术栈准备
  - jQuery：核心架构 事件委托 插件机制
  - Vue React AngularJs NodeJs less sass grunt gulp npm webpack 
  - 可以事先准备些自己熟知的项目、例子、技术点，将面试官往这方面引导
- 自我介绍
  - 简历：基本信息、学历、工作经历、开源项目
    - 基本信息：姓名、年龄、手机、邮箱、籍贯
    - 学历：博士-硕士-本科
    - 工作经历：时间-公司-岗位-职责-技术栈-业绩
    - 开源项目：github和说明
  - 自我陈述
    - 把握面试的沟通方向
    - 豁达、自信的适度发挥
    - 自如谈兴趣、巧妙示实例、适时讨疑问
    - 节奏要适宜、切忌小聪明

---

**一面二面**

- 页面布局
- CSS盒模型
  - 标准模型和IE模型概念 
  - 上述之间区别：content-box border-box
  - CSS如何设置上述两种模型
  - JS如何设置 获取盒模型的宽和高 IE: ele.currentStyle.width/heigh
  - 根据盒模型解释边距重叠
  - BFC（边距重叠解决方案）
    - BFC基本概念：块级格式化上下文
    - 原理，也就是BFC的渲染规则
      - BFC元素垂直方向的边距会发生重叠
      - BFC元素的区域不会与浮动元素的Box重叠
      - BFC在页面是一个独立的容器，外面的元素不会影响里面的元素反之亦然
      - 计算BFC高的时，浮动元素也会参与计算
    - 如何创建：
      - overflow: hidden/auto等, 不是visible
      - float的值不为none
      - position的值不为static 或者relative
      - display 是 table table-cell
    - 使用场景：
- DOM事件
  - 基本概念：DOM事件的级别
![image](https://user-images.githubusercontent.com/20960902/37199182-d7a32bc2-23bb-11e8-8369-c3abd1b42189.png)
  - DOM事件模型
    - 捕获和冒泡
  - DOM事件流
    - 三个阶段
第一阶段：从window对象传导到目标节点，称为“捕获阶段”（capture phase）。
第二阶段：在目标节点上触发，称为“目标阶段”（target phase）。
第三阶段：从目标节点传导回window对象，称为“冒泡阶段”（bubbling phase）。
  - DOM事件的捕获具体流程
  - Event对象的常见应用
event.target：事件最初发生的节点
event.currentTarget：事件当前所在的节点
event.preventDefault()
event.stopPropagation()
event.stopImmediatePropagation()：阻止同一个事件的其他监听函数被调用
  - 自定义事件
```js
// 新建事件实例 如果需要传递数据则需要CustomEvent
var event = new Event('build');

// 添加监听函数
elem.addEventListener('build', function (e) { ... }, false);

// 触发事件
elem.dispatchEvent(event);
```

- Http协议类
  - HTTP协议的主要特点
简单快速：URL对应一个资源
灵活：传个类型，就是请求对应文件类型的资源
无连接：连接一次就会断掉
无状态：不能区分两次连接的身份
  - HTTP报文的组成部分
请求报文：请求行、请求头、空行、请求体
![image](https://user-images.githubusercontent.com/20960902/37249106-a5f12974-251b-11e8-93aa-06e053884e44.png)
相应报文：状态行、响应头、空行、响应体
![image](https://user-images.githubusercontent.com/20960902/37249117-ca6bb9cc-251b-11e8-9112-df63287947bf.png)
  - HTTP的方法
![image](https://user-images.githubusercontent.com/20960902/37249118-d3ca48ee-251b-11e8-8662-372ede0b9894.png)
  - POST和GET的区别
![image](https://user-images.githubusercontent.com/20960902/37249120-e53b2328-251b-11e8-8b6a-5fa9984f33db.png)
记住三到四个
  - HTTP状态码
![image](https://user-images.githubusercontent.com/20960902/37249137-7a01d52e-251c-11e8-9f82-efe1a256c5ca.png)
![image](https://user-images.githubusercontent.com/20960902/37249142-82e51264-251c-11e8-89e6-5a39153f7185.png)
![image](https://user-images.githubusercontent.com/20960902/37249150-ada5de20-251c-11e8-902e-72dac0c2c3d2.png)
  - 什么是持久连接
![image](https://user-images.githubusercontent.com/20960902/37249158-d305ea3e-251c-11e8-97ea-365b92d79753.png)
  - 什么是管线化
![image](https://user-images.githubusercontent.com/20960902/37249161-f2dae9fe-251c-11e8-85b0-3258f13e5a47.png)
![image](https://user-images.githubusercontent.com/20960902/37249166-21bb7ac2-251d-11e8-81c7-f5856635846b.png)
- 原型链类
  - 创建对象几种方法
![image](https://user-images.githubusercontent.com/20960902/37249240-b7cae628-251e-11e8-9899-6ae53045efcb.png)
  - 原型、构造函数、实例、原型链
![image](https://user-images.githubusercontent.com/20960902/37249245-d5b3b052-251e-11e8-975b-e79550efa0a0.png)
  - instanceof的原理
![image](https://user-images.githubusercontent.com/20960902/37249253-2bee3e10-251f-11e8-9a56-a11ec3b80f68.png)
  - new运算符
![image](https://user-images.githubusercontent.com/20960902/37249286-b163e338-251f-11e8-877b-7ad8820708d7.png)
- 面向对象
  - 类与实例
类的声明
生成实例
  - 类与继承
如何实现继承
继承的几种方式
- 通信类
  - 什么是同源策略及限制
  - 前后端如何通信
  - 如何创建Ajax
  - 跨域通信的几种方式
- 安全类
  - CSRF
基本概念：跨站请求伪造
攻击原理
![image](https://user-images.githubusercontent.com/20960902/37249852-7d4d4d18-252a-11e8-938d-7b7fd53d92d4.png)
防御措施：TOKEN验证、Referer验证（页面内来源验证）、隐藏令牌
  - XSS
基本概念：跨域脚本攻击
攻击原理：不需要登录验证，根据合法渠道，注入脚本
防范措施：
- 算法类
  - 排序
![image](https://user-images.githubusercontent.com/20960902/37250012-bf9aa14a-252d-11e8-8501-f848892783eb.png)
![image](https://user-images.githubusercontent.com/20960902/37250013-c70915d8-252d-11e8-8c0e-533f3744fed9.png)
  - 堆栈、队列、链表
![image](https://user-images.githubusercontent.com/20960902/37250019-e402e790-252d-11e8-913b-5f8cf0f4eb25.png)
  - 递归
![image](https://user-images.githubusercontent.com/20960902/37250020-ea562968-252d-11e8-8b5e-61bb6cf0014c.png)
  - 波兰式和逆波兰式
![image](https://user-images.githubusercontent.com/20960902/37250025-08dc2298-252e-11e8-8517-dc606ee39b54.png)
先理解题目（问面试官给个提示，说自己知道用了什么技术点，自己说一下。。）

**二面/三面**

面试技巧：

- 知识面要广
- 理解要深刻
- 内心要诚实
- 态度要谦虚
- 回答要灵活
- 要学会赞美

渲染机制：

什么是DOCTYPE及作用
![image](https://user-images.githubusercontent.com/20960902/37250178-af412a2c-2531-11e8-828c-66251ffb4509.png)

浏览器渲染过程
![image](https://user-images.githubusercontent.com/20960902/37250192-00721abe-2532-11e8-9291-dad40fa29ab3.png)

- 重排Reflow
- 重绘
- 布局

JS运行机制：
![image](https://user-images.githubusercontent.com/20960902/37250312-8341e3dc-2534-11e8-83d3-b2329bd93f49.png)

页面性能：
![image](https://user-images.githubusercontent.com/20960902/37250322-dd701ad6-2534-11e8-8677-974d1f67beb1.png)

缓存：
![image](https://user-images.githubusercontent.com/20960902/37250376-1e592d98-2536-11e8-99c0-8dd6fbe559c6.png)

错误监控：
![image](https://user-images.githubusercontent.com/20960902/37250434-7bec1154-2537-11e8-8e61-1f7cb732d408.png)
![image](https://user-images.githubusercontent.com/20960902/37250470-66f77af8-2538-11e8-9581-ff00a1acdd1f.png)

---

**三面四面**

面试技巧：
- 准备要充分
- 描述要演练（模拟演练）
- 引导找时机（引导说出自己优势，做过的项目啥的）
- 优势要发挥
- 回答要灵活

考查能力：
- 业务能力
  - 主动描述或被动回答
![5-1 mp4_20180806_111002 553](https://user-images.githubusercontent.com/20960902/43695152-738743fc-9969-11e8-9fe5-e2ba76c175f1.jpg)
- 团队协作能力
![up1](https://user-images.githubusercontent.com/20960902/43695482-91e8cec2-996b-11e8-92f3-03e8fccec462.jpg)
- 事务推动能力
![5-2 mp4_20180806_111758 024](https://user-images.githubusercontent.com/20960902/43695491-98908b84-996b-11e8-9c53-8328af8f466f.jpg)
- 带人能力
![5-2 mp4_20180806_111833 048](https://user-images.githubusercontent.com/20960902/43695504-a975cb3a-996b-11e8-8d1c-470993e3bdf4.jpg)
- 其他能力
  - 组织能力
  - 学习能力
  - 行业经验

---

**最终面**

面试技巧：
- 乐观积极
- 主动沟通
- 逻辑顺畅
- 上进有责任心
- 有主张，做事果断

考察问题：
- 职业竞争力
![6-1 mp4_20180806_112528 128](https://user-images.githubusercontent.com/20960902/43695470-72fa6106-996b-11e8-8f4d-db462b0764dd.jpg)
- 职业规划
![6-2 mp4_20180806_112910 520](https://user-images.githubusercontent.com/20960902/43698843-a442ee1e-997e-11e8-8802-1a9938459bbf.jpg)
