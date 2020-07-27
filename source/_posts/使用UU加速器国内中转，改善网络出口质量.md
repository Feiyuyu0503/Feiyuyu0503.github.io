---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 使用UU加速器国内中转，改善网络出口质量
tags:
  - 技术
excerpt: ''
date: 2019-03-28 16:17:31
---

### 写在前面

我们知道UU加速器针对国服是免费的，所以就有了本篇文章。 **注意：此方法仅为拯救长城宽带等质量极差的网络，正常质量的网络就没必要这么搞了。**

### 使用方法

1.打开UU加速器，找一个国服游戏，选择模式一，然后找一个节点连上。 ![img](https://i.loli.net/2020/03/11/wx4yWVkrYq8uKQT.png) 2.更改路由表 管理员模式下打开cmd，执行命令`ipconfig`，查看UU连接详情。 ![img](https://i.loli.net/2020/03/11/EvG8fTFwnadrOc7.png) 执行命令`route add xxx.xxx.xxx.xxx mask 255.255.255.255 10.36.220.12`

    route add xxx.xxx.xxx.xxx mask 255.255.255.255 10.36.220.12
    #添加一条路由：发往xxx.xxx.xxx.xxx这个网段的全部要经过网关10.36.220.12
    route del xxx.xxx.xxx.xxx mask 255.255.255.255
    #删除一条路由，删除时不需要网关
    

### 效果

原本： ![img](https://i.loli.net/2020/03/11/b9oYygRp7GMx26X.jpg) 中转后： ![img](https://i.loli.net/2020/03/11/NJUd1CeQKYhonmO.jpg) 测速： ![img](https://i.loli.net/2020/03/11/vOGjgkU4XiKRpyc.png) ![img](https://i.loli.net/2020/03/11/ZEjeRKTDV3WY8so.jpg)
