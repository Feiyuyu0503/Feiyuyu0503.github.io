---
title: K2P刷机教程
tags:
  - 日志
  - 技术
excerpt: ''
date: 2018-07-15 22:09:51
---

虽然斐讯翻车，799还分文未到账，但不管怎么样，先水一篇文章吧。
================================

前提：斐讯K2P分MTK联发科版本（其中又分A1，A2）和BCM博通版本（B1），本教程使用的是A2。 准备工具：[RoutAckPro](http://www.right.com.cn/forum/forum.php?mod=attachment&aid=MTg4MjU3fDg5ZjdkZmE4fDE1MzE2NTQ2MDR8MHwyNjEwMjg%3D "RoutAckPro"), [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html "Putty"), [官改固件](http://woo.iytc.net/vfm-admin/vfm-downloader.php?q=dXBsb2Fkcy9LMlBfTVRLL2sycF9tdGtfdjE2X2JyZWVkLnJhcg==&h=d2c1ebd50b444deb67b2b4418c30373c "官改固件")/[荒野无灯Padavan固件](http://p4davan.80x86.io/download/ "荒野无灯Padavan固件") 教程开始： 1.使用RoutAckPro激活路由器Telnet 首先确保windows启用Telnet客户端。

![](http://www.feiyuyu.net/wp-content/uploads/2018/08/44a66ee95790e0884e96842e72038302.png)

接着路由器不插wan口，电脑无线连接上路由器或用网线连接lan口和电脑插口（总之需要不联网状态），打开RoutAckPro，重启路由器，当路由器由红灯变为黄灯（绿灯为联网状态），立即点击打开Telnet按钮。

![](http://www.feiyuyu.net/wp-content/uploads/2018/07/e421c5d98e7d3af840c6337b702099fe.png)

2.登录Telnet，刷入官改固件。 使路由器处于联网状态，用windows命令行执行“telnet 192.168.2.1”或用putty等工具连接 ![](http://www.feiyuyu.net/wp-content/uploads/2018/07/8720583e43be81f259a12e0f442264b3.png)![](http://www.feiyuyu.net/wp-content/uploads/2018/07/eb4b8bd2e7800fbd2139e1f84b8a843b.png) 连接成功后，执行如下命令。

    wget http://woo.iytc.net/tools/k2p.sh -O - |sh
    

等待reboot后自动断开连接，此时已刷入官改固件。

![](http://www.feiyuyu.net/wp-content/uploads/2018/07/1e2e1b08b9f99c5599ea7641ffbb70f1.png)

3.登录路由器管理地址，进行初始设置。 浏览器打开192.168.2.1，设置斐讯路由器的管理员帐号，ssid和wifi密码。 点击功能设置页面，可以看到比原版系统新增了不少功能。

![](http://www.feiyuyu.net/wp-content/uploads/2018/07/d6bfce8c16e6b5c6d5bbbc01616cd877.png)

最实用的大概就是广告过滤，KMS激活和魔法上网的功能吧。自行搞定内网穿透后，可远程管理路由器。

4.（可选）进入路由器Breed，刷入荒野无灯的Padavan固件。 电脑网线连接路由器任一lan口，断开路由器电源，按住路由器复位键5s左右（不松手），接通电源，10s后松开复位键。此时访问浏览器192.168.1.1，即进入路由器Breed界面。 点击固件更新，选择以trx为后缀名的padavan固件（版本不同，中间的日期不同），勾选自动重启，闪存选择公版（如果刷回官改则选择斐讯），点击上传后确认更新，等待刷机完成。

![](http://www.feiyuyu.net/wp-content/uploads/2018/07/692535003757f03f3eded510012aae4c.png)

路由器重启完成后，浏览器访问192.168.4.1，帐号密码默认都是admin。然后进行个性化设置。 默认页面是英文的，点击Advanced settings-Administration，最下方Language处可更改为简体中文。 开启彩蛋： 由于某些原因，部分功能是隐藏的，需要自行开启。点击高级设置-系统管理-控制台 分三次输入如下命令(一行一行输入 #后面的注释不用复制 输入一行点一次刷新 注意不会有任何提示 别没看到提示就以为没生效)

    nvram set google_fu_mode=0xDEADBEEF #开启google-fu(即**R)
    nvram set ext_show_lse=1 #开启KMS(可选功能，不知道这个干什么用的就不要输入了）
    nvram commit #（写入固件）
    

![](http://www.feiyuyu.net/wp-content/uploads/2018/07/ec8ddd95f92cd3ffcaff1fc1c1748a82.png)

完成后重启路由器，再次访问路由器管理地址，即可看到隐藏功能被开启了。

总结：由于官改版有Breed，所以先刷入官改固件，可防止刷第三方固件时路由器变砖。Padavan固件很优秀，不过和K2P的契合度还不够（反正极路由的padavan固件体验远胜于K2P），毕竟灯大还在不断更新K2P的padavan固件，尚不如极路由成熟。不过B1版应该会好很多吧，有更多固件可以选择。K2P官改也够用，毕竟硬件不错，youtube可以流畅看4k。极路由可参考我的另一篇文章：[极路由3刷固件Padavan完整教程](http://www.feiyuyu.net/archives/1270 "极路由3刷固件Padavan完整教程")、[斐讯K2路由器刷机教程](http://www.feiyuyu.net/archives/1429 "斐讯K2路由器刷机教程")