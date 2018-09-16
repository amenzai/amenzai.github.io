---
title: SeaJS笔记
categories: 前端学习笔记
tags: 模块化
comment: true
date: 2017-04-06 20:26:07
---
这一篇讲解一下SeaJS。

<!-- more -->

```javascript   
// 包在匿名函数里面，避免污染全局变量
(function() {

    var brian_said = 'hello world',
        ritchie_said_also = 'konicuwa world?';

    // 代码！代码！代码！
})();
```
```javascript   
// 全局变量弥足珍贵，不能用的太豪爽啊
var Precious = {};

// module 1
Precious.mod1 = (function() {
    // 写示例敢不敢不再用 hello world？
    return {
        // 公开方法、变量
    }
})();

// module 2
Precious.mod2 = (function() {
    // 那用日语 ohayo world 行不行？
    return {
        // 公开方法、变量
    }
})();
```

```javascript   
define('console', function(require, exports) {
    exports.log = function(msg) {
        if (window.console && console.log) {
            console.log(msg);
        }
        else {
            alert(msg);
        }
    };
});
```

```javascript   
seajs.use('console', function(console) {
    // now you can stop worrying about whether or not `console` is provided.
    // use it freely!
    console.log('hello world!');

    // well, we need to enhance the `console` module a bit.
    // for methods like console.warn, and method calls like console.log(msg1, msg2, msg3);
});
```

```javascript   
define('jordan', function(require, exports) {
    // 可以内联
    exports.chamionship = function() {
        // 乔丹和皮蓬是对好基友
        return require('pippen').hasJoined();
    };

    // 也可以在头部引入
    var rodman = require('rodman');

    exports.next3 = function() {
        // 大虫
        return require('rodman').hasJoined();
    };
});
```

```javascript   
// 对象
define({ foo: 1 });

// 字符串
define('hello world');
```

```javascript   
<!-- more compact way -->
<script src="sea.js" data-main="./app"></script>
```