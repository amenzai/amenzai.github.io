---
title: ajax笔记
categories: 前端学习笔记
tags: javascript
comment: true
date: 2015-12-24 20:26:07
---
ajax使用总结。

<!-- more -->

# ajax使用代码示例

## 原生写法
```javascript 
<script>
  //第一步：创建xhr对象
  var xhr = null;
  if (window.XMLHttpRequest) { //标准浏览器
    xhr = new XMLHttpRequest();
  } else { //早期的IE浏览器
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
  }

  //第二步：准备发送请求-配置发送请求的一些行为
  var url = '05open.php?username=' + encodeURIComponet(username) + '&password=' + password;
  //var url = '05open.php';
  xhr.open('get', url);

  //第三步：执行发送的动作
  //var param = 'username=' + username + '&password=' + password;
  xhr.send(null);

  //第四步：指定一些回调函数
  xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
      var data = xhr.responseText; //json
      // var data = JSON.parse(this.responseText);
      console.log(data);
      // var data1 = xhr.responseXML;
    }
  }
</script>
```
## jQuery写法
```javascript 
<script>
$.ajax({
  type : "get",
  dataType:"text",
  data:{cityName:'luoyang'},
  url : './05open.php?username=中国&password=123',
  success : function(data){
  console.log(data);
  },
  error: function(e) {
    console.log(e);
  }
});
</script>
```
## 封装的ajax方法
```javascript 
<script>

  // ajax函数
  function ajax(data){
    // data参数的配置
    //data={data:"",dataType:"xml/json",type:"get/post"，url:"",asyn:"true/false",success:function(){},failure:function(){}}

    //data:{username:123,password:456}
    //data = 'username=123&password=456';
    
    //第一步：创建xhr对象
    var xhr = null;
    if(window.XMLHttpRequest){//标准的浏览器
      xhr = new XMLHttpRequest();
    }else{
      xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }


    //第二步：准备发送前的一些配置参数
    var type = data.type == 'get'?'get':'post';
    var url = '';
    if(data.url){
      url = data.url;
      if(type == 'get'){
        url += "?" + data.data + "&_t="+Math.random();
      }
    }
    var flag = data.asyn == 'true'?'true':'false';
    xhr.open(type,url,flag);

    //第三步：执行发送的动作
    if(type == 'get'){
       xhr.send(null);
    }else if(type == 'post'){
       xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
       xhr.send(data.data);
    }

    //第四步：指定回调函数
    xhr.onreadystatechange = function(){
      if(this.readyState == 4){
        if(this.status == 200){
          if(typeof data.success == 'function'){
            var d = data.dataType == 'xml'?xhr.responseXML:xhr.responseText;
            data.success(d);
          }
        }else{
          if(typeof data.failure == 'function'){
            data.failure();
          }
        }
      }
    }
  }

  // 调用方法
  var param = {
    url:'demo.php',
    type:'get',
    dataType:'json',
    success:function(data){
      alert(data);
    }
  };
  ajax(param);
</script>
```
# 跨域问题
在文档里加一个src标签，将需要的js文件引入就行，这其实和ajax没关系。

**原理**：在引入的js文件中调用了一个函数，并进行了传参，这个函数就是我们需要在文档里编写的（同名函数），并且接收参数

**实例**
```javascript 
  <script type="text/javascript">
    function cb(data){
      console.log(data);
    }
    //引入后相当于这样：cb(["zhangsan","lisi","zhaoliu"])
  </script>
  <script type="text/javascript" src="jsonp.php?_jsonp=cb"></script>

  //jsonp.php文件内容
  <?php 

    $callback = $_GET['_jsonp'];

    $arr = array("zhangsan","lisi","zhaoliu");

    echo $callback."(".json_encode($arr).")";

  ?>

**PHP jsonp**
<script>
  $.getJSON("http://www.runoob.com/try/ajax/jsonp.php?jsoncallback=?", function(data) {
  var html = '<ul>';
  for(var i = 0; i < data.length; i++)
  {
  html += '<li>' + data[i] + '</li>';
  }
  html += '</ul>';
  $('#divCustomers').html(html);
  });
</script>
```

# ajax获取xml
```javascript 
var data = xhr.responseXML; //data为xml文档即xmlDoc

var bs = data.getElementsByTagName('bookstore')[0];

var books = bs.getElementsByTagName('book');
```