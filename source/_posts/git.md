---
title: git使用指南
categories: 前端工具
tags: git
comment: true
date: 2016-08-20 20:26:07
---

简单介绍一下git的安装、配置及常用命令。

<!-- more -->

# 安装Git
[官网](https://git-for-windows.github.io/)

# 设置全局变量
```bash
$ git config --global user.name "amenzai" # 提交版本时的用户名

$ git config --global user.email "775166868@qq.com" # 提交版本时的邮箱
```

# 配置SSH
```bash
$ ssh-keygen -t rsa -C "775166868@qq.com"
```
然后可以不输入文件名字 ，直接回车，到`C:\Users\xxxx\.ssh `文件夹下   打开id_rsa.pub文件，复制内容，到github官网，点击用户图像里的设置，点击左侧的  sshkeys，黏贴相关信息。

```bash
$ ssh -T git@github.com # 检查是否添加成功
```

# 初始化一个本地GIT仓储
```bash
$ cd demo-project # 定位到仓储文件夹目录

$ git init # 初始化本地仓储
```

# 添加本地GIT忽略清单文件.gitignore
```bash
# 添加OS X中系统文件.DS_Store到忽略清单，这将忽略项目任意目录下的.DS_Store文件或是文件夹
$ echo .DS_Store >> .gitignore 
```

# 查看本地仓储的变更状态
```bash
$ git status
```

# 添加本地暂存（托管）文件
```bash
$ git add README.md # 添加指定文件名的文件

$ git add *.md # 添加通配符匹配的文件

$ git add --all # 添加所有未托管的文件（忽略.gitignore清单中的列表）
```

# 提交被托管的文件变化到本地仓储
```bash
$ git commit -m 'Initial commit(change log)'
```

# 为仓储添加远端（服务器端）地址
```bash
$ git remote add origin https:# github.com/amenzai/demo-project.git # 添加一个远端地址并起了一个别名叫origin

$ git remote -v # 查看现有的远端列表
```

# 将本地仓储的提交记录推送到远端的master分支
```bash
$ git push -u origin master
```

# 拉取远端master分支的更新记录到本地
```bash
$ git pull origin master
```

# 其他常用命令
```bash
$ git status -s # 输出简要的变更日志

$ git diff # 对比当前状态和版本库中状态的变化

$ git log # 可以查看提交日志

$ git reset --hard xxxxxx # 回归到指定版本

$ git branch # 查看仓库有哪些分支以及当前处于哪个分支

$ git branch V2 # 创建一个V2分支

$ git checkout V2 # 切换到V2分支

$ git push -u origin V2 # 提交到V2分支
```

# 相关网址
- [Github官网](https://github.com/)
- [Github客户端下载](https://desktop.github.com/)
- [GitHub Guides](https://guides.github.com/)
- [Git Guides](http://www.bootcss.com/p/git-guide/)
