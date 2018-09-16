---
title: XML简介
categories: 前端学习笔记
comment: true
date: 2016-02-20 20:26:07
---

# XML用法

## 创建文档。
大家都知道，Xml是一种基于对象的语言，也就是说，在JavaScript中，很多东西也都是面向对象的。可以使用new关键字创建一个对象。当然，创建一个Xml文档也不例外。var xmlDoc = new ActiveXObject("Msxml2.DOMDocument.3.0");

<!-- more -->

## 添加节点。
其实，使用JavaScript操作Xml，和使用C#操作Xml有着几乎相同的方法。我们可以使用C#中一样的方法appendChild()方法来实现添加XML节点。说到这里你该问如何创建Xml节点了，其实还是跟C#相同的方法 var root = xmlDoc.createElement("root"); 将这个root节点添加到xmlDoc这个文档中就很简单了： xmlDoc.appendChild(root); 现在，xmlDoc中就有了一个根节点了。需要注意的是，一个Xml文档只能有一个根节点。其它的节点个数不限。

## 得到节点。
查询节点的方法就很多了。可以使用一个一个节点遍历，也可以使用selectSingleNode()方法或者selectNodes()方法。而得到一个文档根节点的方法则是通过属性documentElement来得到。那么得到xmlDoc的根节点就是 var rootNode = xmlDoc.documentElement;现在我们向root节点中添加一个book节点：var book = xmlDoc.createElement("book"); root.appendChild(bookNode); 那么得到book节点就是：var bookNode = root.selectSingleNode("book");或者var bookNode = root.selectNodes("book")[0]; 或者var bookNode = root.firstNode;你可以使用任何一种方法来得到自己想要的节点。

## 删除节点。
其实这个功能使用量是非常少的。你可以使用removeChild()方法来操作。这里不再详细说明。

## 一些代码示例
```
//根据文本创建xml对象
function createXml(data){
  if(!data){
    return null;
  }
  var xml = null;
  try{
    xml = new ActiveXObject('Microsoft.XMLDOM');
    xml.loadXML(data);
  } catch(e){
    try{
            xml = (new DOMParser()).parseFromString(data,'text/xml'); 
    } catch(e){
          return null;
    }
  }
  return xml;
}

//调用函数 返回xmlDoc
var xml = '<?xml version="1.0" encoding="utf-8"?><student class="高三1班"><username>zhangsan</username><age>12</age></student>';
var obj = createXml(xml);

//获取文本节点内容
function getNodeText(node){
  if(window.ActiveXObject){
    return node.text;
  }else{
    if(node.nodeType == 1){
      return node.textContent;
    }
  }
}

//获取节点的属性
function getNodeAttribute(node,attrName){
  if(window.ActiveXObject){
    return node.getAttribute(attrName);
  }else{
    if(node.nodeType == 1){
      return node.attributes[attrName].value;
    }else{
      return undefined;
    }
  }
}
```
## xml基础示例
```
<!-- html部分 -->
<h1>产品列表</h1>
<div id='div'>

</div>

<script>
    //生XML对象。
    function createXMLDom(){
        if (window.ActiveXObject)
            var xmldoc=new ActiveXObject("Microsoft.XMLDOM");
        else
        if (document.implementation&&document.implementation.createDocument)
            var xmldoc=document.implementation.createDocument("","doc",null);
        xmldoc.async = false;
        //为了和FireFox一至，这里不能改为False;
        xmldoc.preserveWhiteSpace=true;
        return xmldoc;
    }
    //加载XML文件。
    var xmlDom=createXMLDom();
    xmlDom.load("products.xml");
    //获得根节点
    var root=xmlDom.documentElement;
    var data="";
    var names=root.getElementsByTagName("name");
    var ages=root.getElementsByTagName("price");
    var len=names.length;
    for(var i=0;i<len;i++) {
        data+="<strong>产品名称:</strong>";
        data+=names[i].firstChild.nodeValue;
        data+=" <strong>产品价格:</strong>";
        data+=ages[i].firstChild.nodeValue;
        data+=" <br />";
    }
    console.log(data)
    var div = document.getElementById('div')
    div.innerHTML = data;
</script>
```
## xml文档示例
```
<!-- 第一个 -->
<?xml version="1.0" encoding="utf-8" ?>
<item>
    <title>女司机在故障红灯前等40分钟报警求助(01/14 15:13)</title>
    <link>http://news.sina.com.cn/s/2013-01-14/151326030803.shtml</link>
    <author>WWW.SINA.COM.CN</author>
    <category>社会新闻</category>
    <pubDate>2013 07:13:46</pubDate>
    <comments></comments>
    <description>
        1月11日，记者联系到了李女士，她给记者讲述了当时的情况。“我等了40分钟，说到底是担心不遵守交通规则会不安全。”她说。遇红灯不变 女司机坚持等11日10时15分，李女士准备开车出门办事，却发现道路上异常拥堵。“平时我都是11点出门，因为有事当天才出门....
    </description>
</item>

<!-- 第二个 -->
<?xml version="1.0" encoding="gb2312"?>
<products>
    <product id="1">
        <name>iphone7s</name>
        <price>5555</price>
    </product>
    <product id="2">
        <name>iphone7s</name>
        <price>5555</price>
    </product>
    <product id="3">
        <name>iphone7s</name>
        <price>5555</price>
    </product>
    <product id="4">
        <name>iphone7s</name>
        <price>5555</price>
    </product>
    <product id="5">
        <name>iphone7s</name>
        <price>5555</price>
    </product>
</products>

<!-- 第三个 -->
<?xml version="1.0" encoding="utf-8" ?>
<users>
    <user>
        <id>22240319830000</id>
        <name>小李</name>
        <age>26</age>
        <gender>男</gender>
        <email>xiao@hotmail.com</email>
        <phone>13843140000</phone>
    </user>
    - <user>
        <id>22040319860001</id>
        <name>小张</name>
        <age>23</age>
        <gender>女</gender>
        <email>zhang@hotmail.com</email>
        <phone>13843140002</phone>
</user>
</users>
```
## 其他
```
<script type="text/javascript">
    

    var is_Ie = false; //是否为IE浏览器
    if (window.ActiveXObject) {
        is_Ie = true;
    }
    //加载多浏览器兼容的xml文档
    function loadXml(xmlUrl) {
        var xmldoc = null;
        try {
            xmldoc = new ActiveXObject("Microsoft.XMLDOM");
        } catch (e) {
            try {
                xmldoc = document.implementation.createDocument("", "", null);
            } catch (e) {
                alert(e.message);
            }
        }
        try {
            //关闭异步加载
            xmldoc.async = false;
            xmldoc.load(xmlUrl);
            return xmldoc;
        } catch (e) {
            alert(e.message);
        }
        returnnull;
    }
    //将一个xml文档格式的字符串换成xml文档
    function createXml(xmlText) {
        if (!xmlText) {
            returnnull;
            try {
                var xmldocm = new ActiveXObject("Microsoft.XMLDOM");
                xmldocm.loadXML(xmlText);
                return xmldocm;
            } catch (e) {
                try {
                    return new DOMParse().parseFromString(xmlText, "text/xml");
                } catch (e) {
                    return null;
                }
            }
        }
    }
    //获取节点及其子节点的文本
    function getXmlText(oNode) {
        if (oNode.text) { //IE
            return oNode.tex;
        }
        var sText = "";
        for (var i = 0; i < oNode.childNodes.length; i++) { //遍历子节点
            if (oNode.childNodes[i].hasChildNodes()) { //是否有子节点
                sText += getXmlText(oNode.childNodes[i]);
            } else {
                sText += oNode[i].childNodes.nodeValue;
            }
        }
        return sText;
    }

    //获取节点及其子节点的字符串标识
    function getXml(oNode) {
        if (oNode.xml) { //IE
            return oNode.xml;
        }
        var serializer = new XMLSerializer();
        return serializer.serializeToString(oNode);

    }
    //获取指定节点的文本
    function getxmlnodeText(oNode) {
        if (is_Ie) {
            return oNode.text;
        } else {
            if (oNode.nodeType == 1)
                return oNode.textContent;
        }
    }
    //获取指定节点的属性值
    function getxmlnodeattribute(oNode, attrName) {
        if (is_Ie) {
            return oNode.getAttribute(attrName);
        } else {
            if (oNode.nodeType == 1 || oNode.nodeType == "1")
                return oNode.attributes[attrName].value;
            return "undefined";
        }
    } 
</script>
```