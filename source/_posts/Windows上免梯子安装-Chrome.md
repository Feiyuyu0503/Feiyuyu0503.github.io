---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: Windows上免梯子安装 Chrome
tags:
  - 技术
excerpt: ''
date: 2018-10-25 22:55:22
---

看到这个标题，你以为我会让你去哪里找个离线安装包对吗？ No！ 打开你的 PowerShell 愉快的开始使用 Windows 包管理器吧！ 在任务栏 Win 按钮上右键，打开 **Windows PowerShell （管理员）** 输入 `Install-PackageProvider chocolatey` 正常的情况下， Windows 会问你是否安装 **Nuget** 输入 **Y** 回车 然后系统会问你是否信任源，输入 **A** 回车 （或者在每次询问时回答 **Y**） ![PowerShell 源](https://i.loli.net/2017/09/14/59ba2439e88f7.png) 至此你已经启用了 Windows 包管理器并安装 chocolatey 源了 输入 `Install-Package googlechrome` 安装 Chrome 吧！ 全程中文提示，很容易理解哦~ 转自[LetITFly BBS](https://bbs.letitfly.me/d/19 "LetITFly BBS")
