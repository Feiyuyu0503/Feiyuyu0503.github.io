---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 软件
title: IDM + 油猴脚本 实现百度云加速下载
tags:
  - 日志
  - 软件
excerpt: ''
date: 2018-04-06 23:41:49
---

前言：现在国内主流的云存储还是百度云，虽然博主已经放弃百度云了，但还是写一篇教程吧。

原理：单纯使用 **IDM** 和 **“解决百度云大文件下载限制”油猴脚本** 来加速百度云文件的下载，不再需要使用客户端或者任何第三方直链下载工具。

准备工作
----

直接去 [这里](https://d.idm.party/6.BH%E7%A0%B4%E8%A7%A3%E5%AE%89%E8%A3%85%E7%89%88/ "这里") 可以下载到 IDM 的破解版，emm，当然如果你有心支持正版那是更好的（最近得知FDM下载器免费，有兴趣可尝试） 安装完之后打开，如果你用的是 **Chrome** 浏览器，那可能需要安装一个额外的 IDM 支持扩展（会自动提示安装），然后在扩展管理中**勾选“允许访问文件网址”![](http://www.feiyuyu.net/wp-content/uploads/2018/04/3196094521.png)** 安装完后打开主程序，选项-下载，调整线程数为`32（太大可能会导致文件损坏）` ![](http://www.feiyuyu.net/wp-content/uploads/2018/04/1703574932.png)

### 安装油猴和脚本

油猴就是 Tampermonkey，一个非常有名的 js 脚本扩展程序 官网：[https://tampermonkey.net/](https://tampermonkey.net/) 根据你的浏览器选择安装对应版本即可 然后打开 [解决百度云大文件下载限制](https://greasyfork.org/zh-CN/scripts/17800-%E8%A7%A3%E5%86%B3%E7%99%BE%E5%BA%A6%E4%BA%91%E5%A4%A7%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E9%99%90%E5%88%B6 "解决百度云大文件下载限制") 脚本安装页面，安装，一路确定

如何运用
----

进入网页百度云，找到你需要下载的文件（无论是单个还是多个），**放到一个新建的文件夹中**，然后对着这个文件夹点击下载按钮，以文件夹打包的方式下载 ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/f07d22927f32466818a60676b1a97f4f.png)

这样就能摆脱客户端的龟速下载了，正常情况下至少能跑到 5MB/s 以上（已经跑满极速了）当然也看个人带宽情况。

如果你需要下载的文件大小总和超过`4GiB`，那么可能会发生解压的时候说压缩文件夹损坏的情况，**但是你用 7zip 是可以解压出来的**，而且文件实际上并没有损坏 如果你嫌麻烦或者怕产生不测，也可以分小一点控制在`4GiB`以内，如果是单个超过`4GiB`的文件，也可以考虑直接点击下载这单个文件而不是通过文件夹打包的方式，不过这样一般下载速度会比打包慢

**Tips:** 如果是某些热门资源（单个文件），可以考虑在弹出下载框后不要用 IDM 下载，复制 URL 到迅雷中下载，如果有 Hash 相同的(P2P)资源是可以帮助加速下载的，甚至直接高速通道/离线下载
