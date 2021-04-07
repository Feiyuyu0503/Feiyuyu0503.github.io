---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: SSL证书链不完整问题
tags:
  - 技术
excerpt: ''
date: 2019-04-05 15:22:06
---

### 问题描述

最近给博客接入CDN，国外用[Cloudflare](https://www.cloudflare.com/ "Cloudflare")的免费CDN，国内使用[nodecache](https://www.nodecache.com/price "nodecache")的CDN。给nodecache添加好[Let's Encrypt](https://letsencrypt.org/ "Let's Encrypt")证书之后，发现PC上访问博客一切正常，而在Android上用Chrome浏览器访问同样没问题，但是用夸克等浏览器会报“证书过期或不受信任”的错误。

### 原因

**证书链不完整** Android的WebView不能打开页面应该是与这有关，造成这个问题的主要原因是服务器配置证书的证书链不全造成的。 每个设备中都会存有一些默认的可信的根证书，但很多CA是不使用根证书进行签名的，而是使用中间层证书进行签名，因为这样做能更快的进行替换（这句可能不对，原文是 because these can be rotated more frequently）。

如果你的服务器上没有中间件证书，这样的结果就是你的服务器上只有你的网站的证书，客户端的浏览器里只有CA的根证书，这样就会导致证书信任链不全。这种中间层证书不全的问题多出现在移动端的浏览器上（目前来看ios基本没有出现问题，Andorid各个版本都有这个问题）。

当服务器上的证书中的信任链不全的情况下，浏览器会认为当前的链接是一个不安全的，会阻止页面的打开。

### 解决方案

**补全证书链** 1.比较方便的是使用这个在线的工具：[](https://certificatechain.io/ "https://certificatechain.io/")[https://certificatechain.io/](https://certificatechain.io/) 进入这个网站，粘贴进你的证书（只包含你的用户证书），或者上传你的证书，它就会给出补全后的证书文件，你只需要粘贴回你的文件或者下载覆盖就可以了，然后在服务器上重新部署就可以解决问题。

由于这里只需要上传证书，所以是安全的，不需要担心不安全的问题。

如果不喜欢用在线的工具，可以使用下面这个本地的工具，PHP写的，在命令行中运行： [Github ssl-certificate-chain-resolver](https://github.com/spatie/ssl-certificate-chain-resolver "Github ssl-certificate-chain-resolver")

2.Let’s Encrypt 工具生成的证书有以下几个文件 域名.key 证书密钥 域名.cer 域名证书 fullchain.cer 含中间证书的，完整证书链 切换使用fullchain.cer后可恢复正常
