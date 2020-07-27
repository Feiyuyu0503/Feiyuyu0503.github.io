---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 一键网络重装CentOS 7 （官方，纯净，安全，高效）
tags:
  - 技术
excerpt: ''
date: 2018-11-08 21:52:34
---

### 写在前面

只需一键即可重新安装CentOS7系统。（官方，纯净，安全，高效） 无需CD-ROM，无需VNC，只需要一个网络。如果您正在寻找这样的解决方案，那就试试吧。 今天作者带来了更加官方，纯粹，安全高效的CentOS系统。不需要提供商的系统模板直接来自官方的CentOS 7。 同时，本系统针对服务器迁移做了特地优化，可以实现异机迁移，这在一般情况下这是不可能的。 **目前的需求** 我们可以肯定越来越多的人和家庭以及公司开始使用服务器（专用服务器或VPS）。 但是服务提供商提供的系统模板对我们来说并不令人满意。这是不纯的，不安全的，甚至预设了一些软件。许多商家都有内置的监控程序，甚至劫持我们的文件。这就是我们所了解的。 我们不希望看到这种情况，我们希望改进，就像人们的隐私应该受到更多保护一样。

### 使用

在正常模式下运行如下命令

    wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd 'https://opendisk.cxthhhhh.com/OperatingSystem/CentOS/CentOS_7.X_NetInstall_AutoPartition.vhd.gz'
    

### 使用须知

1.  它将帮助您重新安装最新的CentOS7.X系统。（正式，纯粹，安全，高效）
2.  执行后，您可能需要15-45分钟后才能通过IP:22进行连接。
3.  新安装的系统root密码为：cxthhhhh.com
4.  系统首次启动后，请等待自动安装完成，系统将自动重启，然后才能使用。（安装过程中的手动干预可能会导致错误）
5.  IPV4和IPV6是开启的，并通过DHCP获取网络信息。
6.  系统的DNS将被设置为1.1.1.1和8.8.8.8，用来保护您的隐私。
7.  系统使用官方CentOS镜像，将自动匹配yum信息。拒绝服务提供商劫持。
8.  登录信息标准化，易于管理。您需要在登录后修改它。
9.  完成测试，非常适合Azure，Google Cloud，Vultr，Online，Net，OVH，阿里云，腾讯云中的许多专用服务器和KVM服务器。欢迎您的反馈。
10.  对于手动分区版系统模板，默认分配磁盘大小为4.5G。所以你需要扩展磁盘（按照你的需求，决定是否进行扩展磁盘）。
11.  自动分区版将会把所有的硬盘剩余空间分配给（/）根分区，请注意，你只可以在全新的服务器上使用自动分区版，如果你有数据在服务器上，请使用手动分区版（根据需求手动挂载您的数据盘），这是为了防止自动分区版在安装时将您的数据盘格式化。
12.  在每次重装前，请保证你已经通过服务商面板重装过一次系统（CentOS/Debian/Ubuntu均可），不可以在已经DD安装过的系统上使用萌咖的自动脚本再次DD本系统，否则会报错，系统无法启动。手动DD不受此问题影响。
13.  在安装过程中，请勿手动进行操作，也许会导致错误。如果您在屏幕（VNC）上看到以下界面，代表正在安装系统，请耐心等待。 ![CentOS_7.X_NetInstall.png](https://i.loli.net/2019/01/05/5c30326cdbb15.png) 14.腾讯云中国版部分机器（腾讯云国际版机器测试没问题）和DigitalOcean机器请注意，如果安装完无法联网（SSH无法连接）。你应该进入VNC，修改网络配置，设置静态IP。（因为他们的服务商推送了错误的DHCP网关信息，所以自动获取的网络是无效的，无法访问网络。） `vim /etc/sysconfig/network-scripts/ifcfg-eth0 BOOTPROTO="static" #dhcp改为static IPADDR=192.168.1.100 #静态IP GATEWAY=192.168.1.255 #默认网关 NETMASK=255.255.255.0 #子网掩码` 然后(重新设置下一次启动进行自动分区命令) `cp /etc/cxthhhhh.com/first_one.sh /root/first_one.sh` 最后执行`reboot` 本次重启等几分钟再连接，是为了防止影响自动分区，导致出现问题。

### 扩展磁盘

1.  Enter disk management (Please note that your disk is vda or sda, I will use vda to demonstrate how to partition) `fdisk /dev/vda`
2.  Press the keyboard **\[n\]** in sequence to divide the remaining space. Next enter the keyboard **\[p\]**, indicating that we want to create a primary partition. Next enter the keyboard **\[3\]**, indicating that we want to create vda3. The default value is taken twice in succession, so that all remaining space can be divided.
3.  Next press the keyboard **\[t\]** to indicate that we want to modify the partition format. Next enter the serial number **3**, indicating that we want to modify the vda3 space. Next enter the serial number **8e**, indicating that we want to modify to the LVM partition format. Next press the keyboard **\[w\]** to save the changes to the partition table. (ignoring the notification that the device is busy)
4.  Assign the remaining free disk space to the /root partition `partprobe pvcreate /dev/vda3 vgextend centos /dev/vda3`
5.  View the LVM size that can be divided into /root partitions. `vgdisplay` \[Assignable LVM Space\] is the value obtained by multiplying the value of \[Free PE / Size\] by 4MB, and the size is MB.
6.  Assign to / root partition by size. `lvextend -L +[Assignable LVM Space]MB /dev/mapper/centos-root resize2fs -p /dev/mapper/centos-root df -h`

### 写在最后

原文转自：[Technical Blog | 技術博客](https://tech.cxthhhhh.com/linux/2018/07/31/original-network-one-click-reinstall-centos-7-official-pure-safe-efficient-cn.html "Technical Blog | 技術博客") \[Technical Blog | 技術博客\]的Telegram交流频道：[](https://t.me/me_share "https://t.me/me_share")[https://t.me/me\_share](https://t.me/me_share)（TG讨论组群请关注TG频道置顶消息）
