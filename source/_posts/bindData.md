---
title: 数据绑定的相关知识
categories: 前端学习笔记
tags: javascript
comment: true
date: 2015-12-30 20:26:07
---
javascrpt中数据绑定相关内容

<!-- more -->

# 字符串相加方式
获取到数据通过字符串相加，用`element.innerhtml=str`即可。

# 动态创建标签绑定
根据不同的交互情况（比如登录状态）命名不同的函数，创建不同的标签，绑定不同的数据（也可用字符串相加模式省的创建标签），使用jQuery更好。

# formateString
已经封装到框架中，直接调用`$$.formateString(str, data)`

演示：
```javascript   
var user={name:'马院长XXX',role:'钻石会员XXX'}
var span = document.getElementById("span");
span.innerHTML = $$.formateString("欢迎#(name) #(role)来到百度世界",user)
```
# 模板技术
基础用法：
```html
//HTML部分：
<h1>最新上映电影：</h1>
<div id='mydiv'></div>

//JS部分
<!--模板王者演示-->
<script id="arttemplate" type="text/html">
    <strong>电影名称：</strong>{{name}}<br>
    <strong style='color:red'>导演</strong>{{lead}}<br>
    <strong style='color:green'>主演:</strong>{{role}}
</script>

//这是外部引入的js文件，用里面封装好的这两个方法：
<script src='js/itcast.js'></script>

bindTemplate:function (tempalteId,data){
var html = template(tempalteId, data);
return html;
}

<script>
    //标准json定义-- 必须有标题，必须有双引号
    var film = {
        name: "美人鱼",
        lead: "周星驰",
        role: "邓超",
    };
    document.getElementById('mydiv').innerHTML = $$.bindTemplate('arttemplate',film);

</script>

还有一种：
artTemplate:function (str,data){
    var render = template.compile(str);
    return render(data)
}
将模板与数据放到一个script内
<script>
    var film= {name:'美人鱼',lead:'周星驰',role:'邓超'}
    var source = '<strong>{{name}}</strong>'
            +  '<strong>{{lead}}</strong>'
            +  '<strong>{{role}}</strong>'
    document.getElementById('content').innerHTML = $$.artTemplate(source,film);

</script>
```

# replace的用法
```javascript   
//将字母a替换成字母A  正确的写法 /g表示匹配所有
myString = "javascript is a good script language";
console.log('替换全部')
console.log(myString.replace(/a/g,"A"));
```