---
title: js面向对象基础知识杂谈
categories: 前端学习笔记
tags: javascript    
comment: true
date: 2016-01-20 20:26:07
---
js面向对象基础知识杂谈

<!-- more -->

万事万物皆为对象。

编程方式的比较：

传统的编程方式：通过声明变量给变量赋值，然后将其绑定到需要这些值的元素上
element.innerHtml = var;
给相应元素绑定事件

json方式：是把数据存放到json对象里，然后绑定给需要的元素。

面向对象方式：创建用到的对象，对每个对象添加相关的属性和方法。


面向对象编程方式：
先分析需要哪些工具包
然后依次开发每个工具包
然后再使用已经写好的工具包实现我们想要的功能

好处：
将代码分类管理
    将产品相关代码放在一起
    将购物车相关代码放在一起
    使用的时候只需要使用某个工具即可
    将一坨代码用函数包装起来，看做一个整体
代码清晰
容易维护
容易发现问题
代码可读性好
易于团队化作战 – 一个制造工具，一个使用工具
更多好处等待大家发现

开发思想：

面向对象编程就是先把工具包开发出来，这些工具包中包含很多小工具。

然后我们使用一个一个工具将整体功能就像搭积木一样搭建出来，而不是一句一句的，一坨一坨的毫无组织，毫无纪律的编写代码。

开发时，先看需要什么对象，然后对每个对象进行开发，抽象然后实例化。例如列表我们队每个列表进行实例，可以把它们放到数组里进行遍历。

json:
json就是对象的一个实例。直接使用。
后台给我们：json 字符串 xml 二进制数据
json协议 传递数据使用json格式
ajax中，后台给我们传的数据是json字符串（json协议）
JSON.stringify(obj)将JSON转为字符串
通过eval() 函数可以将JSON字符串转化为对象。
JSON.parse(string)将字符串转为JSON对象；

json xml 都是一种通用数据传输协议  都是用来描述数据

json体积小 传递速度快  带宽小 没XML那么通用  可以和JS对象互换

xml: 以XML格式描述一些内容  他是一种规范
Xml只是描述数据的一种结构，比如大家常用的html就是采用这种结构描述的。。
HTML就是XML

面向对象的三个作用：
1. 保存一些常用的工具来简化我们的开发工作（封装功能）
单一职责：解耦合（互不影响）
大的工程拆分成多个模块（架构师要做的），每个模块互不影响。
2. 描述数据
3. 框架

初始化
绑定
交互
目标：
- 理解什么是对象
- 会用面向对象分析网络上常见的网页案例
- 新闻
- 商城
- 教育平台
- 众筹
- P2P
- 面向对象编程
- 对象的三个作用

从生活场景或网站开发场景提炼出对象的属性和方法。

# 开发流程
先宏观思考需要哪些对象，针对每个对象编写其所需的属性和方法。然后实例化对象，定义属性值（所需数据），获取页面DOM元素，然后让DOM元素绑定数据。

获取DOM元素，可以定义为对象的一个属性，绑定数据可以在对象原型里定义这样一个方法。

# 传统编程方式
1. 普通形式
  - 定义变量 - 产品（名称，描述啥的）
  - 获取元素
  - 绑定元素
  - 绑定事件
2. json保存数据
  - 定义编程需要的数据（json保存产品标题描述啥的）
  - 购物车（购物车名称总价格啥的）
  - 获取元素 产品
  - 获取元素 购物车
  - 绑定数据 - 产品
  - 绑定事件 - 产品
  - 绑定数据 - 购物车
  - 绑定事件 - 购物车
3. 对象方式（需要几个工具包）
  - 产品工具包 - 保存和产品有关的工具
  - 购物车工具包 - 保存和购物车有关的工具
  - 绑定各自数据和事件

# 对象的创建方式
```
function Product() {
    this.name ='';
    this.price='';
    this.description = '';
    this.doms = {
      attr: document.getElementById('we'),
      attr: document.getElementById('we'),
      attr: document.getElementById('we')
    }
}
Product.prototype={
  bindDom:function(){
      this.doms.title.innerHTML=this.title;
      this.doms.author.innerHTML=this.author;
      this.doms.date.innerHTML=this.date;
      this.doms.source.innerHTML=this.source;
      this.doms.content.innerHTML=this.content;

  },
    getPrice:function() {
        return this.price
    },
    addToCart:function(){
      alert('haha');
    }
}

var iphone = new Product();
iphone.name = 'aaa';
iphone.name = 'aaa';
```

# 面向对象与列表
```
//产品对象
/*对象内如何使用对象的属性和方法：this，对象外如何使用：先实例化，后用点语法*/
function Product() {
    /*属性 行为*/
    this.name ='';
    this.price='';
    this.description = '';
    this.youhuijia='';
    this.zhekou = ''
    this.sales = ''
    this.image=''
}
Product.prototype={
    bindDom:function(){
        var str=''
        str+='<dl>'
            str+='<dt><a><img src="'+this.image+'" width="384" height="225" /></a></dt>'
            str+='<dd>'
                str+='<span><a><em>'+this.zhekou+'折/</em>'+this.name+'</a></span>'
            str+='</dd>'
            str+='<dd class="price">'
                str+='<em>￥'+this.price+'</em>'
                str+='<del>￥'+this.youhuijia+'</del>'
                str+='<i>售量：'+this.sales+'</i>'
            str+='</dd>'
        str+='</dl>'
        return str;
    },
    bindEvents:function(){

    }
}

/*搭积木开发 -- 代码可读性极高*/
window.onload=function() {
  /*假设这是ajax获取的json数据 -- 假设这是后台给你的数据*/


    /*实例1*/
    var product1 = new Product()
    product1.name = 'SKII'
    product1.price = 1111
    product1.youhuijia = 1000
    product1.sales = 300
    product1.zhekou = 3.5
    product1.image = 'img/boutque10_r2_c2.jpg'

    /*实例2*/
    var product2 = new Product()
    product2.name = '玉兰油'
    product2.price = 1111
    product2.youhuijia = 1000
    product2.sales = 300
    product2.zhekou = 3.5
    product2.image = 'img/boutque10_r2_c2.jpg'

    /*实例3*/
    var product3 = new Product()
    product3.name = '兰蔻'
    product3.price = 1111
    product3.youhuijia = 1000
    product3.sales = 300
    product3.zhekou = 3.5
    product3.image = 'img/boutque10_r2_c2.jpg'



    /*表示有多个产品  我们需要定义多个实例*/
    var products = [product1,product2,product3]

    /*前端代码*/
    /*前后台开发不影响，我们不必等待后端人员给我们数据*/
    var str=''
    for(var i = 0,len=products.length;i<len;i++) {
        str+= products[i].bindDom()
    }
    var container = document.getElementById('container')
    container.innerHTML=str
}
```
# JS面向对象进阶
目标
- 构造函数进阶
- 属性进阶
- 属性进阶2：共有私有属性
- 实例进阶
- 对象的作用进阶
- 使用对象封装框架
- 绑定进阶 - formateString
- 绑定进阶 - 模板技术

## 属性的set get 
```
<script>
    function dateFormat(date,format) {
        var o = {
            "M+" : date.getMonth()+1, //month
            "d+" : date.getDate(),    //day
            "h+" : date.getHours(),   //hour
            "m+" : date.getMinutes(), //minute
            "s+" : date.getSeconds(), //second
            "q+" : Math.floor((date.getMonth()+3)/3),  //quarter
            "S" : date.getMilliseconds() //millisecond
        }
        if(/(y+)/.test(format)) format=format.replace(RegExp.$1,
                (date.getFullYear()+"").substr(4- RegExp.$1.length));
        for(var k in o)if(new RegExp("("+ k +")").test(format))
            format = format.replace(RegExp.$1,
                    RegExp.$1.length==1? o[k] :
                            ("00"+ o[k]).substr((""+ o[k]).length));
        return format;
    }

    //产品对象
    /*对象内如何使用对象的属性和方法：this，对象外如何使用：先实例化，后用点语法*/
    /*类 -- 抽象对象*/
    function Product(name,price) {
        /*属性 行为 可以为空或者给默认值*/
        this.name=name
        this.price=0;
        this.description = '';
        this.zhekou = ''
        this.sales = ''
        this.produceDate
        /*我们的需求：自动计算打折后的价格*/
        Object.defineProperty(this, "price", {
            get: function () {return price*0.9;},
            set: function (value) {
                /*大概普通产品的价格都在0--1万*/
                if(value>10000)
                {
                    alert('产品价格必须在0--1万之间');
                }else{
                    price = value;
                }
            }
        });
        Object.defineProperty(this, "produceDate", {
            get: function () {
                return dateFormat(produceDate,'yyyy-MM-dd');
            },
            set: function (value) {
                produceDate = value;
            }
        });
    }

    //定义对象的两种方式
    Product.prototype={
        getPrice:function() {
            return this.price
        },
        addToCart:function(){
            alert('将物品添加到购物车')
        }

    }


    Product.prototype.buy=function(){

    }
    Product.prototype.addToCart=function(){

    }






    /*获取元素*/
    var btn = document.getElementById('btn')
    var name = document.getElementById('pname')
    var price = document.getElementById('pprice')
    var sum = document.getElementById('sum')
    var des = document.getElementById('pdes')
    var youhuijia = document.getElementById('pyouhui')
    var zhekou = document.getElementById('pzhekou')
    var sales = document.getElementById('psales')
    var date = document.getElementById('date')

    /*搭积木开发 -- 代码可读性极高*/
    window.onload=function() {
        /*实例化 -- 实例 -- 具体*/
        //如何使用
        //对象的使用必须先实例化，对象定义好之后，都是抽象的，必须实例化成具体
        var iphone = new Product()

        /*给对象的赋值赋值，也可以新增属性*/
        iphone.name='iphone7'
        iphone.price=6000
        iphone.description='手机中的战斗机'
        iphone.youhuijia=3000
        iphone.zhekou=3.0
        iphone.sales=40000
        iphone.produceDate=new Date()

        /*绑定元素*/
        /*通过点语法访问对象中的属性或者方法*/
        name.innerHTML=iphone.name
        price.innerHTML=iphone.price
        des.innerHTML=iphone.description
        youhuijia.innerHTML=iphone.youhuijia
        zhekou.innerHTML=iphone.zhekou
        sales.innerHTML=iphone.sales
        date.innerHTML=iphone.produceDate

        /*绑定事件*/
        btn.onclick = function(){
            iphone.addToCart()
        }
    }


</script>
```
## 对象的公有属性-私有属性
```
<script>
    function dateFormat(date,format){
        var o = {
            "M+" : date.getMonth()+1, //month
            "d+" : date.getDate(),    //day
            "h+" : date.getHours(),   //hour
            "m+" : date.getMinutes(), //minute
            "s+" : date.getSeconds(), //second
            "q+" : Math.floor((date.getMonth()+3)/3),  //quarter
            "S" : date.getMilliseconds() //millisecond
        }
        if(/(y+)/.test(format)) format=format.replace(RegExp.$1,
                (date.getFullYear()+"").substr(4- RegExp.$1.length));
        for(var k in o)if(new RegExp("("+ k +")").test(format))
            format = format.replace(RegExp.$1,
                    RegExp.$1.length==1? o[k] :
                            ("00"+ o[k]).substr((""+ o[k]).length));
        return format;
    }

    //产品对象
    /*对象内如何使用对象的属性和方法：this，对象外如何使用：先实例化，后用点语法*/
    /*类 -- 抽象对象*/
    function Product(name,price) {
        /*属性 行为 可以为空或者给默认值*/
        this.name=name
        this.price=1000;
        this.description = '';
        this.zhekou = ''
        this.sales = ''
        this.produceDate
        /*我们的需求：自动计算打折后的价格*/
        Object.defineProperty(this, "price", {
            value:5000000,
            writable: false,
        });
        Object.defineProperty(this, "produceDate", {
            get: function () {
                return dateFormat(produceDate,'yyyy年MM月dd日');
            },
            set: function (value) {
                produceDate = value;
            }
        });
        var that = this;
        this.config = {
            btn: document.getElementById('btn'),
            name :  document.getElementById('pname'),
            price :  document.getElementById('pprice'),
            sum :  document.getElementById('sum'),
            des :  document.getElementById('pdes'),
            youhuijia :  document.getElementById('pyouhui'),
            zhekou :  document.getElementById('pzhekou'),
            sales :  document.getElementById('psales'),
            date :  document.getElementById('date')
        }
        function bindDOM(){
            /*绑定元素*/
            /*通过点语法访问对象中的属性或者方法*/
            that.config.name.innerHTML=that.name
            that.config.price.innerHTML=that.price
            that.config.des.innerHTML=that.description
            that.config.youhuijia.innerHTML=that.youhuijia
            that.config.zhekou.innerHTML=that.zhekou
            that.config.sales.innerHTML=that.sales
            that.config.date.innerHTML=that.produceDate
        }
        function bindEvents(){
            /*绑定事件*/
            that.config.btn.onclick = function(){
                that.addToCart()
            }
        }
        this.init = function(){
            bindDOM()
            bindEvents()
        }
    }

    //定义对象的两种方式
    Product.prototype={

        getPrice:function() {
            return this.price
        },
        addToCart:function(){
            alert('将物品添加到购物车')
        }
    }


    Product.prototype.buy=function(){

    }
    Product.prototype.addToCart=function(){

    }


    /*搭积木开发 -- 代码可读性极高*/
    window.onload=function() {

        /*实例化 -- 实例 -- 具体*/
        //如何使用
        //对象的使用必须先实例化，对象定义好之后，都是抽象的，必须实例化成具体
        var iphone = new Product()

        /*给对象的赋值赋值，也可以新增属性*/
        iphone.name='iphone7'
        iphone.price=6000
        iphone.description='手机中的战斗机'
        iphone.youhuijia=3000
        iphone.zhekou=3.0
        iphone.sales=40000
        iphone.produceDate=new Date()


        /*无法使用私有成员*/
//        iphone.bindDOM()
//        iphone.bindEvents()
        /*只能使用共有成员*/
        iphone.init()
    }


</script>
```
## 数据类型检测
```
var num = 1
var str = '传智播客'
var bool=false;
var arr=[];
var obj={name:'传智播客'};
var date = new Date();
var fn = function(){}

console.log(typeof "abc")  //'string'

console.log(toString.call(123))          //[object Number]

console.log(Object.prototype.toString.call(str) === '[object String]')   //-------> true;

console.log(arr instanceof Array)     //---------------> true

console.log(date.constructor === Date)   //-----------> true
```
## 原型对象进阶
```
<!-- 删除本对象拷贝的对象 -->
delete product.name
//然后就可以访问到原型对象里的name属性了

//hasOwnProperty : 看是不是对象自身下面的属性
function Product(){
    this.name='iphone'
}

Product.prototype={
    age:100
}

var iphone = new Product()
console.log(iphone.hasOwnProperty('name'))
console.log(iphone.hasOwnProperty('age'))

//给对象扩充功能
//在JS源码 : 系统对象也是基于原型的程序
 function Array(){
 this.lenglth = 0;
 }
 Array.prototype.push = function(){};
 Array.prototype.sort = function(){};*/
//尽量不要去修改或者添加系统对象下面的方法和属性
```
## 面向对象总结
- 封装框架
  - 将一些常用的函数功能封装起来
- 面向对象编程
  - 定义对象，编写属性和方法
- 数据描述
  - 常用数据绑定：字符串相加方式，动态创建标签绑定，formateString，模板技术

引用类型、值类型、堆和栈、内存的生命周期、多种方式创建对象

# 多种方式创建对象
- 工厂模式：
```
<script>

    /*一般游戏名称是我们自己取的，其他事默认值*/
    /*那么怎么做呢？*/
    function createPerson(name){
        //1.原料
        var obj = new Object();
        //2.加工
        obj.name = name;
        obj.HP=100;
        obj.MP=100;
        obj.technologys=['普通攻击','横扫千军','地狱火','漫天飞雪'];
        obj.attack = function(){
            alert(obj.name+'发出攻击，导致对方10点伤害值')
        };
        obj.run = function(){
        };
        //3.出场
        return obj;

    }

    var boy = createPerson('剑侠客');
    boy.attack();
    boy.run()


    var girl = createPerson('炫舞天使');
    girl.attack();
</script>

```
- 构造函数-原型对象：

因为在原型对象中引用类型的值存在问题，就是一个对象将其更改，则其他对象都会受影响。

- 字面量形式
- 拷贝模式：
```
//拷贝一个
<script>
    //-------------------------拷贝创建对象核心代码--------------------------

    /*函数的用处：就是将一个json对象 所有属性拷贝给另外一个对象*/
    /*source：原始对象*/
    /*target：目标对象*/
    function extend(target,source) {
        //遍历对象
        for(var i in source){
            target[i] = source[i];
        }
        return target;
    }


    //游戏随机生成名字
       var boy = {
        name:'郭靖'
        ,image:'男性头像'
        ,age:20
        ,sex:'男'
    };

    var girl = {
        name:'黄蓉'
        ,age:18
        ,image:'女性头像'
        ,sex:'女'
    };

    /*六大神器之一*/

    var zuixiake = extend({}, boy);
    var huangrong = extend({},girl)
    alert(zuixiake.name);
    alert(zuixiake.sex);
    console.log(huangrong.name)
</script>

//拷贝多个
<script>

    //extend2实现的功能：extend(target,obj1,obj2,obj3)
    /*功能：将多个多个json拷贝给目标*/
    /*原理：*/
    /*首先找到target   --arguments[0]*/
   function extend () {
        var key,i = 0,len = arguments.length,target = null,copy;
        if(len === 0){
            return;
        }else if(len === 1){
            target = this;
        }else{
            i++;
            target = arguments[0];
        }
        for(; i < len; i++){
            for(key in arguments[i]){
                copy = arguments[i][key];
                target[key] = copy;
            }
        }
        return target;
    }

    /*hasOwnProperty*/
    function extend2(){
        for (var p in source) {
            if (source.hasOwnProperty(p)) {
                target[p] = source[p];
            }
        }

        return target;
    }


    //游戏随机生成名字
    var boy = {
        name:'无忌'
        ,image:'男性头像'
        ,age:20
        ,sex:'男'
    };


    //技能名称，等级，伤害值，需要的魔法
    var technology = {tname:'亡灵复活',tlevel:10,tstrength:3000,tmagic:30};

    var shenqi = {sname:'霜之哀伤',slevel:30,sstrength:3000}
    //当这个人有了穿上盔甲，圣衣，六神合体，戴上魔法戒指之后，自动也拥有一个技能


    //第一种用法
    var zuixiake = extend({}, technology,shenqi);
    zuixiake.name='醉侠客';
    alert(zuixiake.name);
    alert(zuixiake.tname);
    alert(zuixiake.sname);


    //第二种用法
    extend(boy,technology,shenqi);
    alert(boy.name);
    alert(boy.tname);
    alert(boy.sname);

</script>
//使用封装的框架
//第一种用法  醉侠客加入丐帮之后学到的技能
var zuixiake = $$.extendMany({}, gaibang,shaolin);
zuixiake.name='醉侠客';
console.log(zuixiake.name);
zuixiake.XL18.attack()
zuixiake.DLJGS.attack()

//第二种用法  无忌加入丐帮
$$.extendMany(boy,gaibang,shaolin);
console.log(boy.name);
boy.XL18.attack()
boy.DLJGS.attack()
```
- 用第三方框架
```
var Person = Class.extend({
    init: function(isDancing){
        this.dancing = isDancing;
    },
    dance: function(){
        return this.dancing;
    }
});

其他参考案例
```
## 继承
```
Number.__proto__ === Function.prototype  // true
Boolean.__proto__ === Function.prototype // true
String.__proto__ === Function.prototype  // true
Object.__proto__ === Function.prototype  // true
Function.__proto__ === Function.prototype //true 
Array.__proto__ === Function.prototype   // true
RegExp.__proto__ === Function.prototype  // true
Error.__proto__ === Function.prototype   // true
Date.__proto__ === Function.prototype    // true

```