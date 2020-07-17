---
title: 极路由3刷固件Padavan完整教程
tags:
  - 技术
excerpt: ''
date: 2018-10-01 22:08:30
---

### 写在前面\[toc\]

因为家里的极1s不支持5G，斐讯官改用着也不太舒服，所以去闲鱼收了个极路由3（HC5861），一天感觉下来，体验非常不错。

### 准备工具\[toc\]

[Winscp](https://winscp.net/eng/index.php "Winscp")、[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html "Putty")、[Breed引导](https://breed.hackpascal.net/ "Breed引导")（极3对应的文件是breed-mt7620-hiwifi-hc5861.bin）、[固件下载](http://opt.cn2qq.com/padavan/ "固件下载")（极3对应的文件是RT-AC1200HP-GPIO-12-JI3-128M\_3.4.3.9-099.trx）、网线。

### 刷机过程\[toc\]

**开通极路由root权限** 1.浏览器输入192.168.199.1（或hiwifi.com）登录路由器后台（密码默认为wifi密码）。出于好奇，申请root权限前，我看了一下极路由的云插件都有哪些。 ![2.png](https://i.loli.net/2018/10/01/5bb2190aa2fe2.png) ![3.png](https://i.loli.net/2018/10/01/5bb2190aa8a6a.png) 2.进入后台界面中的“智能插件”菜单，前往插件市场，点击“路由器信息”，然后点开底部的“高级设置”，点击【申请】。登陆小极帐号，此时会弹出一个“开发者模式申请声明”的网页，拉到页面底部同意条款。（若路由器已绑定别的小极帐号，可换绑。）而后会要求你绑定微信，最后得到root权限的批准后，需要安装上开发者模式插件，root才算完成。 **刷入breed** 此步骤目的是防止刷机变砖。 1.打开Winscp，文件协议选择“SCP”，主机名填写192.168.199.1，端口号填写1022，用户名填写root，密码为管理后台的登录密码，点击登录，弹窗点“是”。 2.默认显示root目录，然后返回上级找到tmp目录，把下载好的breed-mt7620-hiwifi-hc5861.bin上传到tmp目录。 3.打开Putty，Host name填写192.168.199.1,Port填写1022，点击下方的Open,弹窗点“是”。然后界面会显示“login as: ”，我们输入root，回车，再输入自己极路由后台密码，并回车，登录成功后显示hiwifi图案。 ![login.png](https://i.loli.net/2018/10/01/5bb21ec5c3a44.png) 4.最后执行命令`mtd -r write /tmp/breed-mt7620-hiwifi-hc5861.bin u-boot`，大约等待一两分钟，界面会显示"rebooting"，然后会弹窗“连接中断”，我们等待一会让路由器重启。（可尝试ping 192.168.199.1帮助我们判断是否重启完成） **进入breed刷机** 1.断开路由器电源，用网线连接路由器lan口和电脑网口，按住路由器reset按钮不松手，接通电源，稍等5秒左右后，松手并用浏览器打开192.168.1.1，进入breed界面。 2.**固件备份（强烈建议）。如果以后要刷回官方固件，最好做一下这步。因为极路由有个key的概念，刷回官方最好在breed里还原备份好的固件，如果直接从网上下载官方固件，云插件将无法使用。同时也记录下原来路由器的MAC地址。** 3.点击固件更新，固件选择文件RT-AC1200HP-GPIO-12-JI3-128M\_3.4.3.9-099.trx，勾选自动重启。等待上传好后点击更新，刷入固件。 ** **![breed.jpg](https://i.loli.net/2018/10/01/5bb2269335372.jpg) ![breed.png](https://i.loli.net/2018/10/01/5bb226a4ef92f.png)** ** ******登陆Padavan管理后台** 1.浏览器打开192.168.123.1，默认用户名和密码均为admin。 2.修改系统设置中管理员的信息，修改外部网络信息，修改2.4G/5GHz无线网络信息等自定义操作。**** ****![padavan.png](https://i.loli.net/2018/10/01/5bb227c5e1ff7.png)

### 写在最后\[toc\]

Padavan具备很强大的功能，如ssr，去广告、建立家庭私有云、Aria2下载、web服务器等，基本上路由器刷机都是这样大同小异的操作，斐讯可参考我的另一篇文章：[K2P刷机教程](http://www.feiyuyu.net/archives/996 "K2P刷机教程")、[斐讯K2路由器刷机教程](http://www.feiyuyu.net/archives/1429 "斐讯K2路由器刷机教程")****