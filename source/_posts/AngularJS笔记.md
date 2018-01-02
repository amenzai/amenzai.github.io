---
title: AngularJS笔记
categories: 前端学习笔记
tags: AngularJS
comment: true
date: 2017-04-08 20:26:07
---
这一篇讲解一下AngularJS。

<!-- more -->

# 简介

AngularJS 是一个 JavaScript 框架。它可通过 `<script> `标签添加到 HTML 页面。

AngularJS 通过 指令 扩展了 HTML，且通过 表达式 绑定数据到 HTML。

# 基础

## 模块（module）

AngularJS很重要的一个特性就是实现模块化编程，我们可以通过以下方式创建一个模块，对页面进行功能业务上的划分。

也可以将重复使用的指令或过滤器之类的做成模块便于复用。

注意必须指定第二个参数，否则变成找到已经定义的模块。
```javascript 
var myApp = angular.module("MyApp", []);
```

## 控制器（controller）

调度逻辑的集合。

控制器常用注入：'$scope', '$location', 'MainService', '$routeParams','$http','$window','$document'

```javascript
var app = angular.module('myApp', []);
app.controller('personCtrl', ['$scope',function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
    $scope.fullName = function() {
        return $scope.firstName + " " + $scope.lastName;
    }
}]);
```
控制器的三个主要职责：
1. 为应用中的模型设置初始状态
2. 通过$scope对象把数据模型或函数行为暴露给视图
3. 监视模型的变化，做出相应的动作

代码示例：
```javascript
$scope.$watch(‘totalCart’, calculateDiscount);

$scope.$watch('user.username', function(now, old) {
        // 当user.username发生变化时触发这个函数
        // console.log('now is ' + now);
        // console.log('old is ' + old);
        if (now) {
          if (now.length < 7) {
            $scope.message = '输入格式不合法';
          } else {
            $scope.message = '';
          }
        } else {
          $scope.message = '请输入用户名';
        }
      });
```

## $scope（上下文模型）

- 视图和控制器之间的桥梁
- 用于在视图和控制器之间传递数据
- 利用$scope暴露数据模型（数据，行为）

谷歌插件：`AngularJS batarang`调试`$scope`

## 表达式

使用表达式把数据绑定到html，类似 `ng-bind` 指令。

表达式可以包含文字、运算符、变量，支持过滤器。

语法：
```
{{expression}}
```
建议更多的使用指令


## 指令

AngularJS 有一套完整的、可扩展的、用来帮助 Web 应用开发的指令集。

在 DOM 编译期间，和 HTML 关联着的指令会被检测到，并且被执行。

在 AngularJS 中将前缀为 ng- 这种属性称之为指令，其作用就是为 DOM 元素调用方法、定义行为绑定数据等。

简单说：当一个 Angular 应用启动，Angular 就会遍历 DOM 树来解析 HTML，根据指令不同，完成不同操作。

指令如下：

### ng-app

- ng-app指令用来标明一个AngularJS应用程序
- 标记在一个AngularJS的作用范围的根对象上
- 系统执行时会自动的执行根对象范围内的其他指令
- 可以在同一个页面创建多个ng-app节点

### ng-cloak
添加一下样式，并且在body中添加此属性。
```html
<style>
  [ng\:cloak],
  [ng-cloak],
  [data-ng-cloak],
  [x-ng-cloak],
  .ng-cloak,
  .x-ng-cloak,
  .ng-hide:not(.ng-hide-animate) {
    display: none !important;
  }

  ng\:form {
    display: block;
  }

  .ng-animate-shim {
    visibility: hidden;
  }

  .ng-anchor {
    position: absolute;
  }
</style>

<body ng-app ng-cloak>
```

### ng-repeat

ng-repeat指令用来编译一个数组重复创建当前元素，如果数组中有重复的项，则需要早后面加`track by $index`如
```html
<ul class="messages">
    <li ng-repeat="item in messages track by $index">
        {{item}}
    </li>
</ul>
```

### ng-class

ng-class指令可以设置一个键值对，用于决定是否添加一个特定的类名，键为class名，值为bool类型表示是否添加该类名。
```html
<ul class="messages">
    <li ng-repeat="item in messages track by $index" ng-class="{red:item.read}">
        {{item.content}}
    </li>
</ul>
```

### ng-show/ng-hide

ng-show/ng-hide指令会根据属性值去确定是否展示当前元素，例如ng-show=false则不会展示该元素。
```html
<ul class="messages">
    <li ng-repeat="item in messages track by $index" ng-show="item.read">
        {{item.content}}
    </li>
</ul>

```

### ng-link/ng-src

ng-link/ng-src指令用于解决当链接类型的数据绑定时造成的加载BUG，如
```html
<!-- 浏览器在解析HTML时会去请求{{item.url}}文件 -->
<img src="{{item.url}}">
<!-- 可以使用ng-src解决该问题 -->
<img ng-src="{{item.url}}">
```

### ng-switch
```html
<body ng-app>
  <select ng-model="selected">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>
  <div ng-switch="selected">
    <div ng-switch-when="1">
      你选择的是1
    </div>
    <div ng-switch-when="2">
      你选择的是2
    </div>
    <div ng-switch-when="3">
      你选择的是3
    </div>
    <div ng-switch-default>
      你什么都没选
    </div>
  </div>  
```


### 其他常用命令

- ng-model
- ng-click
- ng-dbclick
- ng-if：指是否存在DOM元素
- ng-checked
- ng-selected
- ng-disabled
- ng-readonly
- ng-blur
- ng-change
- ng-focus
- ng-submit

### 自定义指令

```html
var demoApp = angular.module('demoApp', []);

// 第一个参数是指令的名字，第二个参数任然应该使用一个数组，数组的最后一个元素是一个函数
// 定义指令的名字，应该使用驼峰命名法
demoApp.directive('itcastButton', [function() {
  // 该函数应该返回一个指令对象
  return {
    template:'<input type="button" value="itcast" class="btn btn-lg btn-primary btn-block" />'
  };
}]);
<!-- html中使用 -->
<itcast-button></itcast-button>

//详细说明
demoApp.directive('breadcrumb', [function() {
  // Runs during compile
  return {
    // 指定当前指令的类型什么样的
    // restrict: 'EA',
    // // E = Element, A = Attribute, C = Class, M = Comment
    // template: '', // 模版字符串
    templateUrl: 'tmpls/breadcrumb.html',
    replace: true,
    // transclude: true,
  };
}]);

// demoApp.directive('btn', [function() {
//   return{
//     scope:{
//       primary:'@',
//       lg:'@',
//       block:'@',
//     },
//     template:'<button class="btn {{primary==\'true\'?\'btn-primary\':\'\'}}">button</button>'
//   }
// }]);

// demoApp.directive('btn', [function() {
//   return {
//     // 指令对象的transclude必须设置为true才可以在模版中使用ng-transclude指令
//     transclude: true,
//     replace: true, // 替换指令在HTML中绑定的元素
//     template: '<button class="btn btn-primary btn-lg" ng-transclude></button>'
//   };
// }]);
```
# AngularJS过滤器

过滤器的主要用途就是一个格式化数据的小工具，一般用于服务端存储的数据转换为用户界面可以理解的数据。

## currency	
格式化数字为货币格式。
```
{{ (quantity * price) | currency }}
```

## filter	
从数组项中选择一个子集，其中test是我们在文本框中输入的值进行过滤。
```html
<p><input type="text" ng-model="test"></p>
<ul>
  <li ng-repeat="x in names | filter:test | orderBy:'country'">
    {{ (x.name | uppercase) + ', ' + x.country }}
  </li>
</ul>
```

## lowercase	

格式化字符串为小写。
```
{{ lastName | lowercase }}
```

## orderBy	
根据某个表达式排列数组。
```html
<ul>
  <li ng-repeat="x in names | orderBy:'country'">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
```

## uppercase
格式化字符串为大写。
```
{{ lastName | uppercase }}
```

## 其他
```
{{'itcast.cn,itcast.com,itheima.com' | limitTo:10:10}}

{{p1 | json:8}}

{{1288323623006 | date:'fullDate'}}

{{1112123.141592635 | number:10}}
```

## 自定义过滤器

```javascript 
<p>你{{weight|weight}}</p>

angular.module('app', [])
      .filter('checkmark', function() {
        return function(input, style) {
          style = style || 1; // 短路运算符
          switch (style) {
            case 1:
              return input ? '\u2713' : '\u2718';
            case 2:
              return input ? '\u2714' : '\u2719';
          }
        };
      })
      .filter('weight', function() {
        return function(input) {
          if (input > 100) {
            return '太胖了';
          } else {
            return '太瘦了';
          }
        };
      });
```

# AngularJS表单

# AngularJS服务

- 公用（公共）的业务逻辑集中存放的一段代码
- 主要用于对重复业务的封装，重用
- 一般主要封装针对于Model的CRUD

## 创建服务

```javascript 
var myApp = angular.module('MyApp', []);
// 通过factory方法创建一个公用的service
var userService = myApp.service('UserService', function() {
    var users = { 1: 'zhangsan1', 2: 'zhangsan2' };
    return {
        getUser: function(id) {
            return users[id];
        },
        addUser: function(id, name) {
            users[id] = name;
        },
    };
});
```
## $location服务

类似`window.location`对象，使用`$location.path()`获取`url`中锚点值后的字符串。

## $http服务

$http服务是AngularJS中处理AJAX的服务。
```javascript
// Simple GET request example:
$http({
  method: 'GET',
  url: '/someUrl'
}).then(function successCallback(response) {
    // this callback will be called asynchronously
    // when the response is available
  }, function errorCallback(response) {
    // called asynchronously if an error occurs
    // or server returns response with an error status.
  });

// get
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http.get("welcome.htm").then(function (response) {
        $scope.myWelcome = response.data;
    });
});
```

## $timeout服务

```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $timeout) {
    $scope.myHeader = "Hello World!";
    $timeout(function () {
        $scope.myHeader = "How are you today?";
    }, 2000);
});
```

## $interval服务

```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $interval) {
    $scope.theTime = new Date().toLocaleTimeString();
    $interval(function () {
        $scope.theTime = new Date().toLocaleTimeString();
    }, 1000);
});
```

# AngularJS路由

请求过来，判断用哪个控制器来处理。
```html
  <div ng-view></div>
  <script id="a_tmpl" type="text/ng-template">
    <!-- 只有type="text/javascript"的script节点才会被当做JS执行 -->
    <!-- script中的内容就算不能执行，也不可以显示在界面上 -->
    <h1>{{title}}</h1>
  </script>
  <script>
    var app = angular.module('app', ['ngRoute']);
    app.config(['$routeProvider', function($routeProvider) {
      $routeProvider
      // 某一类特定地址
        .when('/students/:name?', {
          controller: 'StudentsController',
          templateUrl: 'a_tmpl'
        })
        .when('/a', {
          controller: 'AController',
          templateUrl: 'a_tmpl'
        })
        .when('/b', {
          controller: 'BController',
          templateUrl: 'a_tmpl'
        })
        .when('/c', {
          controller: 'CController',
          templateUrl: 'a_tmpl'
        })
        // 别的请求
        .otherwise({
          // 跳转到一个地址
          redirectTo: '/a'
        });
    }]);

    app.controller('StudentsController', ['$scope', '$routeParams', function($scope, $routeParams) {
      $scope.title = '你好' + $routeParams['name'] + '这是A控制器';
    }]);

    app.controller('AController', ['$scope', function($scope) {
      $scope.title = '这是A控制器';
    }]);

    app.controller('BController', ['$scope', function($scope) {
      $scope.title = '这是B控制器';
    }]);

    app.controller('CController', ['$scope', function($scope) {
      $scope.title = '这是C控制器';
    }]);
  </script>
```

# 依赖注入

依赖注入（Dependency Injection，简称DI）是一种软件设计模式，在这种模式下，一个或更多的依赖（或服务）被注入（或者通过引用传递）到一个独立的对象（或客户端）中，然后成为了该客户端状态的一部分。

该模式分离了客户端依赖本身行为的创建，这使得程序设计变得松耦合，并遵循了依赖反转和单一职责原则。与服务定位器模式形成直接对比的是，它允许客户端了解客户端如何使用该系统找到依赖。

AngularJS 提供很好的依赖注入机制。以下5个核心组件用来作为依赖注入：

## value

Value 是一个简单的 javascript 对象，用于向控制器传递值（配置阶段）：
```javascript
// 定义一个模块
var mainApp = angular.module("mainApp", []);

// 创建 value 对象 "defaultInput" 并传递数据
mainApp.value("defaultInput", 5);
...

// 将 "defaultInput" 注入到控制器
mainApp.controller('CalcController', function($scope, CalcService, defaultInput) {
   $scope.number = defaultInput;
   $scope.result = CalcService.square($scope.number);
   
   $scope.square = function() {
      $scope.result = CalcService.square($scope.number);
   }
});
```

## factory

factory 是一个函数用于返回值。在 service 和 controller 需要时创建。
通常我们使用 factory 函数来计算或返回值。
```javascript
// 定义一个模块
var mainApp = angular.module("mainApp", []);

// 创建 factory "MathService" 用于两数的乘积 provides a method multiply to return multiplication of two numbers
mainApp.factory('MathService', function() {
   var factory = {};
   
   factory.multiply = function(a, b) {
      return a * b
   }
   return factory;
}); 

// 在 service 中注入 factory "MathService"
mainApp.service('CalcService', function(MathService){
   this.square = function(a) {
      return MathService.multiply(a,a);
   }
});
```

## provider
AngularJS 中通过 provider 创建一个 service、factory等(配置阶段)。

Provider 中提供了一个 factory 方法 get()，它用于返回 value/service/factory。
```javascript
// 定义一个模块
var mainApp = angular.module("mainApp", []);
...

// 使用 provider 创建 service 定义一个方法用于计算两数乘积
mainApp.config(function($provide) {
   $provide.provider('MathService', function() {
      this.$get = function() {
         var factory = {};  
         
         factory.multiply = function(a, b) {
            return a * b; 
         }
         return factory;
      };
   });
});
```

## constant
constant(常量)用来在配置阶段传递数值，注意这个常量在配置阶段是不可用的。
```
mainApp.constant("configParam", "constant value");
```

# 传参和获取参数

## angular中

例如：/:status

控制器注入$routeParams，用$routeParams.status可获取对应参数。

更新url地址参数，匹配路由:

控制器中注入$route,使用$route.updateParams({ category: 'search', q: $scope.input })方法可以更新url中的参数。

## ionic中

例如：/:typeNumber

控制器注入$stateParams，用$stateParams.typeNumber可获取对应参数。

跳转如下：

控制器注入$state。

```
//用在控制器中

$state.go('goodsList',{typeNumber:666});

//用在视图中
ui-sref="goodsList({typeNumber:{{item.typeNumber}}})”

//直接跳转
<a href="#/goodsList/34">跳转到商品详细页面</a>
```

其实$state.go 与ui-sref一样，后面的参数是路由的名称。

# 项目目录及代码示例

一般主页面会有一个`<ng-view></ng-view>`标签，用来放置各类子页面

## todomvc
只有一个页面，在服务中编写好对应的方法，然后控制器调用并向视图暴露数据和方法。

处理好模块依赖，一般都让主模块依赖控制器模块和服务模块，然后依次编写控制器模块和服务模块。

熟练各种指令，了解获取路由的参数。

项目目录：
```
css
    app.css //个人定制css
js 
    app.js //主模块，并且配置路由
    controllers.js //控制器，调度逻辑
    services.js //各种服务，curd操作
index.html // 入口文件
```

## moviecat

如果页面布局相似，直接抽象出一个模块公用，在此用了列表模块公用。

项目目录：
```
css
    app.css //custom style
movie_list
    controller.js // 配置路由，写逻辑
    view.html
movie_detail
    controller.js 
    view.html
components
    auto-focus.js 
    http.js 
app.js //主模块，并且配置路由。
```
注意主模块中依赖顺序。主模块中配置常量在控制器里可以直接注入，主模块中定义的控制器要在视图中用指令绑定上去，并且只执行一次，如果需执行多次，请执行$watch()方法监控$scope中暴露出的对应数据。

抽象出的共用模块，配置路由时使用参数的形式：/:category/:page

$scope.$apply(); // 跨域请求数据以后，使用此方法将数据绑定到视图。

## 代码示例

**1. app.js**
```javascript 
'use strict';

// Declare app level module which depends on views, and components
angular.module('moviecat', [
    'ngRoute',
    'moviecat.movie_detail',
    'moviecat.movie_list',
    'moviecat.directives.auto_focus',
  ])
  // 为模块定义一些常量
  .constant('AppConfig', {
    pageSize: 10,
    listApiAddress: 'http://api.douban.com/v2/movie/',
    detailApiAddress: 'http://api.douban.com/v2/movie/subject/'
  })
  .config(['$routeProvider', function($routeProvider) {
    $routeProvider.otherwise({ redirectTo: '/in_theaters/1' });
  }])
  .controller('SearchController', [
    '$scope',
    '$route',
    'AppConfig',
    function($scope, $route, AppConfig) {
      $scope.input = ''; // 取文本框中的输入
      $scope.search = function() {
        // console.log($scope.input);
        $route.updateParams({ category: 'search', q: $scope.input });
      };
    }
  ]);


// .controller('NavController', [
//   '$scope',
//   '$location',
//   function($scope, $location) {
//     $scope.$location = $location;
//     $scope.$watch('$location.path()', function(now) {
//       if (now.startsWith('/in_theaters')) {
//         $scope.type = 'in_theaters';
//       } else if (now.startsWith('/coming_soon')) {
//         $scope.type = 'coming_soon';
//       } else if (now.startsWith('/top250')) {
//         $scope.type = 'top250';
//       }
//       console.log($scope.type);
//     });
//   }
// ])

```

**2. movie_list.js**
```javascript 
(function(angular) {
  'use strict';

  // 创建正在热映模块
  var module = angular.module(
    'moviecat.movie_list', [
      'ngRoute',
      'moviecat.services.http'
    ]);
  // 配置模块的路由
  module.config(['$routeProvider', function($routeProvider) {
    $routeProvider.when('/:category/:page', {
      templateUrl: 'movie_list/view.html',
      controller: 'MovieListController'
    });
  }]);

  module.controller('MovieListController', [
    '$scope',
    '$route',
    '$routeParams',
    'HttpService',
    'AppConfig',
    function($scope, $route, $routeParams, HttpService, AppConfig) {
      var count = AppConfig.pageSize; // 每一页的条数
      var page = parseInt($routeParams.page); // 当前第几页
      var start = (page - 1) * count; // 当前页从哪开始
      // 控制器 分为两步： 1. 设计暴露数据，2. 设计暴露的行为
      $scope.loading = true; // 开始加载
      $scope.subjects = [];
      $scope.title = 'Loading...';
      $scope.message = '';
      $scope.totalCount = 0;
      $scope.totalPages = 0;
      $scope.currentPage = page;
      HttpService.jsonp(
        AppConfig.listApiAddress + $routeParams.category,
        // $routeParams 的数据来源：1.路由匹配出来的，2.?参数
        { start: start, count: count, q: $routeParams.q },
        function(data) {
          $scope.title = data.title;
          $scope.subjects = data.subjects;
          $scope.totalCount = data.total;
          $scope.totalPages = Math.ceil($scope.totalCount / count);
          $scope.loading = false;
          $scope.$apply();
          // $apply的作用就是让指定的表达式重新同步
        });

      // 暴露一个上一页下一页的行为
      $scope.go = function(page) {
        // 传过来的是第几页我就跳第几页
        // 一定要做一个合法范围校验
        if (page >= 1 && page <= $scope.totalPages)
          $route.updateParams({ page: page });
      };
    }
  ]);
})(angular);


// var doubanApiAddress = 'http://api.douban.com/v2/movie/in_theaters';
// // 测试$http服务
// // 在Angular中使用JSONP的方式做跨域请求，
// // 就必须给当前地址加上一个参数 callback=JSON_CALLBACK
// $http.jsonp(doubanApiAddress+'?callback=JSON_CALLBACK').then(function(res) {
//   // 此处代码是在异步请求完成过后才执行（需要等一段时间）
//   if (res.status == 200) {
//     $scope.subjects = res.data.subjects;
//   } else {
//     $scope.message = '获取数据错误，错误信息：' + res.statusText;
//   }
// }, function(err) {
//   console.log(err);
//   $scope.message = '获取数据错误，错误信息：' + err.statusText;
// });

```
**3. movie_detail.js**

```javascript 
(function(angular) {
  'use strict';

  // 创建正在热映模块
  var module = angular.module(
    'moviecat.movie_detail', [
      'ngRoute',
      'moviecat.services.http'
    ]);
  // 配置模块的路由
  module.config(['$routeProvider', function($routeProvider) {
    $routeProvider.when('/detail/:id', {
      templateUrl: 'movie_detail/view.html',
      controller: 'MovieDetailController'
    });
  }]);

  module.controller('MovieDetailController', [
    '$scope',
    '$route',
    '$routeParams',
    'HttpService',
    'AppConfig',
    function($scope, $route, $routeParams, HttpService, AppConfig) {
      $scope.movie = {};
      $scope.loading = true;
      var id = $routeParams.id;

      var apiAddress =
        AppConfig.detailApiAddress + id;

      // 跨域的方式
      HttpService.jsonp(apiAddress, {}, function(data) {
        $scope.movie = data;
        $scope.loading = false;
        $scope.$apply();
      });
    }
  ]);
})(angular);
```

**4. auto-focus.js自定义指令**

```javascript 
(function(angular) {
  angular.module('moviecat.directives.auto_focus', [])
    .directive('autoFocus', ['$location', function($location) {
      // Runs during compile
      // var path = $location.path(); // /coming_soon/1
      // console.log(path);
      return {
        restrict: 'A', // E = Element, A = Attribute, C = Class, M = Comment
        link: function($scope, iElm, iAttrs, controller) {

          $scope.$location = $location;
          $scope.$watch('$location.path()', function(now) {
            // 当path发生变化时执行，now是变化后的值
            var aLink = iElm.children().attr('href');
            var type = aLink.replace(/#(\/.+?)\/\d+/, '$1'); // /coming_soon
            if (now.startsWith(type)) {
              // 访问的是当前链接
              iElm.parent().children().removeClass('active');
              iElm.addClass('active');
            }
          })


          // iElm.on('click', function() {
          //   iElm.parent().children().removeClass('active');
          //   iElm.addClass('active');
          // });
        }
      };
    }]);
})(angular);
```
   
**5. http.js跨域服务**

```javascript

  // 由于默认angular提供的异步请求对象不支持自定义回调函数名
  // angular随机分配的回调函数名称不被豆瓣支持
  var http = angular.module('moviecat.services.http', []);
  http.service('HttpService', ['$window', '$document', function($window, $document) {
    // url : http://api.douban.com/vsdfsdf -> <script> -> html就可自动执行
    this.jsonp = function(url, data, callback) {
      // if (typeof data == 'function') {
      //   callback = data;
      // }
      var querystring = url.indexOf('?') == -1 ? '?' : '&';
      for (var key in data) {
        querystring += key + '=' + data[key] + '&';
      }

      var fnSuffix = Math.random().toString().replace('.', '');
      var cbFuncName = 'my_json_cb_' + fnSuffix;
      querystring += 'callback=' + cbFuncName;

      var scriptElement = $document[0].createElement('script');
      scriptElement.src = url + querystring;
      // 不推荐
      $window[cbFuncName] = function(data) {
        callback(data);
        $document[0].body.removeChild(scriptElement);
      };
      $document[0].body.appendChild(scriptElement);
    };
  }]);
```



