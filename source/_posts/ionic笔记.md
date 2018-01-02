---
title: Ionic笔记
categories: 前端学习笔记
tags: Ionic
comment: true
date: 2017-04-10 20:26:07
---
这一篇讲解一下Ionic。

<!-- more -->

# 简介
ionic 是一个强大的 HTML5 应用程序开发框架(HTML5 Hybrid Mobile App Framework )。 可以帮助您使用 Web 技术，比如 HTML、CSS 和 Javascript 构建接近原生体验的移动应用程序。

ionic 主要关注外观和体验，以及和你的应用程序的 UI 交互，特别适合用于基于 Hybird 模式的 HTML5 移动应用程序开发。

ionic是一个轻量的手机UI库，具有速度快，界面现代化、美观等特点。为了解决其他一些UI库在手机上运行缓慢的问题，它直接放弃了IOS6和Android4.1以下的版本支持，来获取更好的使用体验。

# 安装

## 常规安装

CDN、github下载、官网下载的等。
下载后的目录：
```c
css/               =>     样式文件
fonts/             =>     字体文件
js/                =>     Javascript文件
version.json       =>     版本更新说明
```
接下来，我们只需要在项目中引入以上目录中的 css/ionic.min.css 和 js/ionic.bundle.min.js 文件即可创建 ionic 应用。

## 命令行安装

确保配置好对应环境（安卓为例）：
- node
- git
- java_jdk
- android_sdk

```bash
# 安装
$ npm install -g cordova ionic
# 更新
$ npm update -g cordova ionic
# 创建一个模板
$ ionic start myApp tabs
# 进入对应目录
$ cd myApp
# 添加平台
$ ionic platform add android
# 打包
$ ionic build android
# 运行在模拟器中
$ ionic emulate android
# 一般打包完我们用数据线链接手机，输入一下命令
$ adb devices // 测试设备是否链接成功
# 将打包的APK自动安装到手机
$ ionic run android
```

# 基本的模板目录

```c
index.html //入口文件
-css
-js
	app.js //主模块
	services.js
	controllers.js
	routes.js
-img
-lib
	-ionic
- templates
```

## index.html内容示例
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no, width=device-width">
    <title></title>
    <link rel="manifest" href="manifest.json">
    <!-- un-comment this code to enable service worker
    <script>
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('service-worker.js')
          .then(() => console.log('service worker installed'))
          .catch(err => console.log('Error', err));
      }
    </script>-->
    <link href="lib/ionic/css/ionic.css" rel="stylesheet">
    <link href="css/style.css" rel="stylesheet">
    <!-- IF using Sass (run gulp sass first), then uncomment below and remove the CSS includes above
    <link href="css/ionic.app.css" rel="stylesheet">
    -->
    <!-- ionic/angularjs js -->
    <script src="lib/ionic/js/ionic.bundle.js"></script>
    <!-- cordova script (this will be a 404 during development) -->
    <script src="cordova.js"></script>
    <!-- your app's js -->
    <script src="js/app.js"></script>
    <script src="js/controllers.js"></script>
    <script src="js/services.js"></script>
</head>

<body ng-app="starter">
    <!--
      The nav bar that will be updated as we navigate between views.
    -->
    <ion-nav-bar class="bar-stable">
        <ion-nav-back-button>
        </ion-nav-back-button>
    </ion-nav-bar>
    <!--
      The views will be rendered in the <ion-nav-view> directive below
      Templates are in the /templates folder (but you could also
      have templates inline in this html file if you'd like).
    -->
    <ion-nav-view></ion-nav-view>
</body>

</html>
```

## tabs.html

```html
<ion-tabs class="tabs-icon-top tabs-color-active-positive">

  <!-- Dashboard Tab -->
  <ion-tab title="Status" icon-off="ion-ios-pulse" icon-on="ion-ios-pulse-strong" href="#/tab/dash">
    <ion-nav-view name="tab-dash"></ion-nav-view>
  </ion-tab>

  <!-- Chats Tab -->
  <ion-tab title="Chats" icon-off="ion-ios-chatboxes-outline" icon-on="ion-ios-chatboxes" href="#/tab/chats">
    <ion-nav-view name="tab-chats"></ion-nav-view>
  </ion-tab>

  <!-- Account Tab -->
  <ion-tab title="Account" icon-off="ion-ios-gear-outline" icon-on="ion-ios-gear" href="#/tab/account">
    <ion-nav-view name="tab-account"></ion-nav-view>
  </ion-tab>

</ion-tabs>
```
## tab-demo.html

```html
<ion-view view-title="Dashboard">
  <ion-content class="padding">
    <h2>Welcome to Ionic</h2>
    <p>
      This is the Ionic starter for tabs-based apps. For other starters and ready-made templates, check out the <a href="http://market.ionic.io/starters" target="_blank">Ionic Market</a>.
    </p>
    <p>
      To edit the content of each tab, edit the corresponding template file in <code>www/templates/</code>. This template is <code>www/templates/tab-dash.html</code>
    </p>
    <p>
      If you need help with your app, join the Ionic Community on the <a href="http://forum.ionicframework.com" target="_blank">Ionic Forum</a>. Make sure to <a href="http://twitter.com/ionicframework" target="_blank">follow us</a> on Twitter to get important updates and announcements for Ionic developers.
    </p>
    <p>
      For help sending push notifications, join the <a href="https://apps.ionic.io/signup" target="_blank">Ionic Platform</a> and check out <a href="http://docs.ionic.io/docs/push-overview" target="_blank">Ionic Push</a>. We also have other services available.
    </p>
  </ion-content>
</ion-view>
```

### app.js主模块

```javascript 
angular.module('starter', ['ionic', 'starter.controllers', 'starter.services'])

.run(function($ionicPlatform) {
  $ionicPlatform.ready(function() {
    // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
    // for form inputs)
    if (window.cordova && window.cordova.plugins && window.cordova.plugins.Keyboard) {
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
      cordova.plugins.Keyboard.disableScroll(true);

    }
    if (window.StatusBar) {
      // org.apache.cordova.statusbar required
      StatusBar.styleDefault();
    }
  });
})

.config(function($stateProvider, $urlRouterProvider) {

  // Ionic uses AngularUI Router which uses the concept of states
  // Learn more here: https://github.com/angular-ui/ui-router
  // Set up the various states which the app can be in.
  // Each state's controller can be found in controllers.js
  $stateProvider

  // setup an abstract state for the tabs directive
    .state('tab', {
    url: '/tab',
    abstract: true,
    templateUrl: 'templates/tabs.html'
  })

  // Each tab has its own nav history stack:

  .state('tab.dash', {
    url: '/dash',
    views: {
      'tab-dash': {
        templateUrl: 'templates/tab-dash.html',
        controller: 'DashCtrl'
      }
    }
  })

  .state('tab.chats', {
      url: '/chats',
      views: {
        'tab-chats': {
          templateUrl: 'templates/tab-chats.html',
          controller: 'ChatsCtrl'
        }
      }
    })
    .state('tab.chat-detail', {
      url: '/chats/:chatId',
      views: {
        'tab-chats': {
          templateUrl: 'templates/chat-detail.html',
          controller: 'ChatDetailCtrl'
        }
      }
    })

  .state('tab.account', {
    url: '/account',
    views: {
      'tab-account': {
        templateUrl: 'templates/tab-account.html',
        controller: 'AccountCtrl'
      }
    }
  });

  // if none of the above states are matched, use this as the fallback
  $urlRouterProvider.otherwise('/tab/dash');

});
```

## controllers.js

```javascript 
angular.module('starter.controllers', [])

.controller('DashCtrl', function($scope) {})

.controller('ChatsCtrl', function($scope, Chats) {
  // With the new view caching in Ionic, Controllers are only called
  // when they are recreated or on app start, instead of every page change.
  // To listen for when this page is active (for example, to refresh data),
  // listen for the $ionicView.enter event:
  //
  //$scope.$on('$ionicView.enter', function(e) {
  //});

  $scope.chats = Chats.all();
  $scope.remove = function(chat) {
    Chats.remove(chat);
  };
})

.controller('ChatDetailCtrl', function($scope, $stateParams, Chats) {
  $scope.chat = Chats.get($stateParams.chatId);
})

.controller('AccountCtrl', function($scope) {
  $scope.settings = {
    enableFriends: true
  };
});
```

## services.js

```javascript 
angular.module('starter.services', [])

.factory('Chats', function() {
  // Might use a resource here that returns a JSON array

  // Some fake testing data
  var chats = [{
    id: 0,
    name: 'Ben Sparrow',
    lastText: 'You on your way?',
    face: 'img/ben.png'
  }, {
    id: 1,
    name: 'Max Lynx',
    lastText: 'Hey, it\'s me',
    face: 'img/max.png'
  }, {
    id: 2,
    name: 'Adam Bradleyson',
    lastText: 'I should buy a boat',
    face: 'img/adam.jpg'
  }, {
    id: 3,
    name: 'Perry Governor',
    lastText: 'Look at my mukluks!',
    face: 'img/perry.png'
  }, {
    id: 4,
    name: 'Mike Harrington',
    lastText: 'This is wicked good ice cream.',
    face: 'img/mike.png'
  }];

  return {
    all: function() {
      return chats;
    },
    remove: function(chat) {
      chats.splice(chats.indexOf(chat), 1);
    },
    get: function(chatId) {
      for (var i = 0; i < chats.length; i++) {
        if (chats[i].id === parseInt(chatId)) {
          return chats[i];
        }
      }
      return null;
    }
  };
});
```

# Ionic CSS文档

参考地址：http://www.ionic.wang/css_doc-index.html

# Ionic JS文档

参考地址：http://www.ionic.wang/js_doc-index.html

# Ionic图标

参考地址：http://ionicons.com/

# 常用网址

- [ionic 官方网站](http://ionicframework.com/)
- [ionic 官方文档](http://ionicframework.com/docs/)
- [Github 地址](https://github.com/driftyco/ionic)

# 项目文件代码示例

## shopping

入口文件编写规则：

head中，引框架带的css，插件css，各个功能模块的css，自己定制css

body底部，引入框架带的js，插件js，全局js，各个功能模块js

一般每个功能模块对应一个页面或者多个相同页面不同数据，每个功能模块编写对应的路由、控制器、服务、视图。

全局js：

- app.js：主模块，依赖['ionic','route','global','config','ionicLazyLoad']
- config.js：配置兼容
- global.js：全局常量配置
- route.js：总路由 依赖各个功能模块路由

各个模块路由依赖对应的控制器模块，各自控制器模块依赖服务模块。

```javascript 
angular.module('guidePage.services', [])
  .factory('GuidePageFty', function($http,$q) {
    return {
      get:function(){},
      postdata:function(){},
      deletedata:function(){}
    };
});
```