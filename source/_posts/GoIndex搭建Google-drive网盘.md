---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: GoIndex搭建Google drive网盘
tags:
  - 技术
excerpt: ''
date: 2019-09-14 20:00:49
---

### 写在前面

GoIndex是OneIndex项目作者利用Cloudflare Workers开发的google drive文件目录程序。利用GoIndex可以实现无墙访问google drive内的文件，同时部署也非常简单，成本也几乎为0。 项目地址：[GoIndex](https://github.com/donwa/goindex "GoIndex")

### 部署教程

**1.访问[GoIndex 代码生成](https://install.gd.workers.dev/ "GoIndex 代码生成")** 1.1 点击获取认证码 选择登陆你要部署成goindex的谷歌账号，允许rclone访问，将生成的认证码粘贴至Auth认证码。 1.2填写目录id 打开google drive(某文件夹或共享文件夹)，看地址栏。 [](https://drive.google.com/drive/folders/{这里就是id)[](https://drive.google.com/drive/folders/{这里就是id)[](https://drive.google.com/drive/folders/{这里就是id)[https://drive.google.com/drive/folders/{这里就是id](https://drive.google.com/drive/folders/{这里就是id)} 留空为根目录。 1.3设置根目录密码 1.4点击生成代码 复制代码粘贴到cloudflare workers中（详见下方第2步） ![](http://www.feiyuyu.net/wp-content/uploads/2019/09/7aa7ced28603892c47c2576ab8ee9da4.png) **2.登陆[cloudflare](https://dash.cloudflare.com "cloudflare")** 2.1点击new Workers dashboard ![](http://www.feiyuyu.net/wp-content/uploads/2019/09/ffaa70aff1bbaabedaf05393d86051dc.png) 2.2注册你的subdomain 点击Create a worker。进入详情页，在Script里粘贴第1步最后生成的代码。此时右边会实时显现效果。 ![](http://www.feiyuyu.net/wp-content/uploads/2019/09/353dbe760315896444c6597dd668ea6d.png) 2.3保存 选择Deployed to your workers.dev subdomain，并点击Save and Deploy。此时就可以通过访问刚才注册的subdomain，访问部署好的GoIndex了。 **3.自定义域名** 如果不想用cloudflare的免费域名.workers.dev,可以绑定你的自定义域名。 提前解析好你的域名，选择Workers，点击Add route，填写域名，主要最后加/\*， 选择刚部署好的Worker，点击save。 ![](http://www.feiyuyu.net/wp-content/uploads/2019/09/0768d092954873da66f5f9cdf5b1a8ca.png) 详情请查看https://github.com/donwa/goindex/issues/4
