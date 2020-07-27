---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 国行Android内置Google框架无法登陆账号的解决
tags:
  - 技术
excerpt: ''
date: 2019-04-05 02:05:21
---

### 问题描述

如果在路由器端设置了基于“**国内 IP 白名单**”或 **GFWList** 的智能代理 可能会在自带 Google 框架的**国行** Android 手机(原厂 ROM) 上 登录 Google Play 时会遇到 **DF-DFERH-01** 错误 在手机/平板开启 Shadowsocks(R) / V2Ray 客户端时，一般不会出现此错误 ![](https://i.loli.net/2020/03/11/tB7KOpZu3VUkWIw.jpg)![](https://i.loli.net/2020/03/11/uUSYQib28DV6vMK.jpg)

### 原因分析

国行 Android 设备内置的 Google 框架会请求 **services.googleapis.cn** 而不是 **services.googleapis.com** 域名，而前者在中国大陆境内会被解析到来自北京的服务器，然而 Google Play 还没有回归中国。

### 解决方案

在路由器上把 **services.googleapis.cn** 强行解析到海外 IP，目前是 **172.217.0.74** _如果未来有变，此域名解析到的最新海外 IP 可在 **[ping.pe](http://ping.pe/services.googleapis.com "ping.pe")** 查询到_ 如果你的路由器也在用 **Pavadan**固件 ，可以在 `内部网络(LAN) -> DHCP 服务器 -> 高级设置` 中 DHCP 服务器详细: 选择 **DHCPv4** 然后在 `自定义配置文件 "dnsmasq.servers"` 中加入

    server=/services.googleapis.cn/172.217.0.74
    

最后在 `自定义配置文件 "hosts"` 中加入

    172.217.0.74 services.googleapis.cn
    

然后保存设置即可。 其实方法应该蛮多的，改hosts，添加例外规则等都可以尝试下 转自[LetITFly BBS](https://bbs.letitfly.me/d/860 "LetITFly BBS")
