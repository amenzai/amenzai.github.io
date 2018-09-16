---
title: SublimeText安装及常用插件推荐
categories: 前端工具
tags: ide
comment: true
date: 2015-07-30 20:26:07
---
SublimeText可谓是前端工程师的代码编辑神器，用了一定会爱上她。

<!-- more -->

# SublimeText安装

[官网下载](http://www.sublimetext.com/3)根据自己的电脑系统下载相应的版本。

# SublimeText 插件安装

安装之前我们需要先安装package control组件。

## package control安装

### 在线安装

[点击此处](https://packagecontrol.io/installation#st3)选择左边SublimeText3栏目下面的代码复制，在SublimeText编辑器中按 ctrl+` 调出console面板，进行粘贴。

### 手动安装

点击Preferences 选择Browse Packages ，打开其上一级文件夹Data并进入Installed Packages文件夹，将下载的[Package Control.sublime-package](https://packagecontrol.io/Package%20Control.sublime-package)文件复制进去，再重启SublimeText。

## 插件安装

### 在线安装

按下Ctrl+Shift+P调出命令面板，输入install选择Install Package 选项并回车，再输入你要安装的插件名称(例如sublime tmpl)，然后在列表中选中要安装的插件。

安装成功后一般在Perferences->package settings中可看到，有些插件有可能在列表中搜索不到，你可以选择手动安装。

### 手动安装

进入[sublimetext安装包管理器官网](https://packagecontrol.io/)，在搜索框里输入你所需要的插件名称。

进入插件的github页面（点击Details下面的github.com），点击右侧的clone or download -> Download ZIP，将下载的压缩包解压后放在Preferences->Browse Packages（即packages文件夹）里面，并重命名（去掉文件名中的##master），重启Sublimetext3即可安装成功。

# SublimeText插件推荐

## emmet

让html css的编写更加迅速。[语法参考](http://docs.emmet.io/cheat##sheet/)

## ConvertToUTF8

文件转码成utf##8

## IMESupport

sublime中文输入法

## Nodejs

node代码提示

## AutoFileName

快捷输入文件名

## Doc​Blockr

生成优美注释

## jQuery

jQ函数提示

## Bracket Highlighter

可匹配[]， ()， {}， “”， ”， <tag></tag>，高亮标记，便于查看起始和结束标记

## LESS

LESS高亮插件
## JSFormat

JavaScript的代码格式化插件

## Color Highlighter

快捷选取颜色选中还有颜色显示

## CSS Comments

各种css注释快捷书写

## HTML-CSS-JS Prettify

格式化上述类型文件

## markdownpreview

可以预览markdown文件，以及生成HTML页面。

## Minifier

压缩JS和CSS文件

## SideBarEnhancements

侧边目录树内容更加丰富

## SublimeTmpl

新建模板文件

## File Header

生成文件头，记录更新情况。

以上这些基本上就是前端常用的插件，使用方法可以自行百度(太懒就木有写使用方法0.0)
