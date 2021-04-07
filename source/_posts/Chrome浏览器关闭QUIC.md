---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 漫谈
title: Chrome浏览器关闭QUIC
tags:
  - 漫谈
excerpt: ''
date: 2019-03-18 20:25:51
---

### QUIC是什么

**QUIC（Quick UDP Internet Connection）**是谷歌制定的一种基于 UDP 的低时延互联网传输层协议。 因为TCP协议连接建立的成本相对较高，但是可以通过TCP快速打开（TCP Fast Open）来减少建立连接时的握手次数。但是该技术目前应用较少。 和TCP相反，UDP协议是无连接协议。客户端发出UDP数据包后，只能“假设”这个数据包已经被服务端接收。这样的好处是在网络传输层无需对数据包进行确认，但存在的问题就是为了确保数据传输的可靠性，应用层协议需要自己完成包传输情况的确认。 与 TCP 协议相比，UDP 更为轻量，但是错误校验也要少得多。这意味着 UDP 往往效率更高（不经常跟服务器端通信查看数据包是否送达或者按序），但是可靠性比不上 TCP。通常游戏、流媒体等应用均采用 UDP，而网页、邮件、远程登录等大部分的应用均采用 TCP。 此时，QUIC协议就登场了。QUIC协议可以在1到2个数据包（取决于连接的服务器是新的还是已知的）内，完成连接的创建。

### 为什么要关闭

因为 QUIC 为了实现 UDP 的高效，会把一些 TCP 转为 UDP，但是**在国内部分地区的运营商都会针对 UDP 协议QOS限速或者丢包，**这就导致 **UDP 效率低下**，或许速度会比正常使用TCP协议还慢很多。 而谷歌的服务器，例如 Google搜索、Youtube等，都部署了 QUIC 服务，这意味着当你使用**已开启 QUIC 功能的基于Chromium内核浏览器**访问谷歌网站的时候，会尝试使用 QUIC 方式传输数据。而**碰巧你当地运营商对 UDP协议歧视**，然后疯狂限速或丢包，这时候你的速度就会很感人。 **注意：各地区的运营商对 UDP协议的态度不一样，有的地区QOS严重，有的地区则很轻，所以关闭 QUIC 只对部分地区用户会有加速效果！** 又或者你使用酸酸乳代理，而服务端没有开启 UDP 转发功能(或者防火墙没开放 UDP)，那么你可能会遇到打开 优土鳖视频后，视频会一直缓冲无法加载，或者是首次打开总是慢很多（因为浏览器在尝试）。 **注意：QUIC适用于一切基于 Chromium 的浏览器，如果不是这个内核的浏览器就没必要看了。**

### 如何关闭QUIC

首先打开你基于Chromium内核的浏览器，地址栏输入： `chrome://flags/#enable-quic` 然后就会看到如下图，在下拉框中可以选择 **默认/已启用/已禁用 (Default/Enabled/Disabled)** 三个选项，我们只需要把选项改为 **已禁用(Disabled)** 即可。 修改后，需要重启浏览器生效。 ![img](https://i.loli.net/2020/03/11/KVIeLhzRNyp1uUX.png)

转自：[https://51.ruyo.net/5262.html](https://51.ruyo.net/5262.html) [https://www.echoteen.com/turnoff-quic.html](https://www.echoteen.com/turnoff-quic.html)
