---
title: MI 4 LTE 一次坎坷的刷机经历
tags:
  - 日志
  - 技术
excerpt: ''
date: 2018-08-02 18:54:59
---

就结论而言，这次刷机是失败的，毕竟我想刷的是Android P，结果不得已停留在了Android 8.1。原因是刷入P的前提是底包必须是支持Project Treble的Rom，而我所刷入的底包经过检测，并不支持PT。（如何检测可通过：[Treble Check](https://play.google.com/store/apps/details?id=com.kevintresuelo.treble "Treble Check")） 准备工具：[刷机包（密码up64）](https://pan.baidu.com/share/init?surl=qXVMRDy "刷机包（密码up64）")、[ADB工具包（密码wttk）](https://pan.baidu.com/s/1lGeG8MWxq-1nlRuWaGLwFw "ADB工具包（密码wttk）")、[电信基带补丁（密码1max）](https://pan.baidu.com/s/1n6KFEi07FDhkcOUYIqpbcA "电信基带补丁（密码1max）")

### 刷入第三方REC

首先USB连接手机与电脑，确保手机进入fastboot（关机时长按电源键+音量下键），电脑上将刷机包中提供的recovery.img压缩为zip文件，打开ADB工具包的文件夹，按shift+鼠标右键，打开shell命令窗口，在窗口输入：fastboot flash recovery ，并拖动recovery.zip到此命令窗口（此时会自动读取文件路径），回车等待刷入第三方rec。 注意：此REC并非官方提供的小米4的TWRP，为Rom修改版。经博主实际测试，官方TWRP刷入此Rom，会出现Error 7。（博主遇到的第一个坑，而后作了更大的死）

### 进入Recovery刷入Rom

关机时长按电源键+音量上键，进入recovery。wipe双清（cache、data）。 卡刷： 提前将刷机包放入手机目录下，包括Aosp-Oero包和Gapps包。点击install，分别选择Aosp包和Gapps包刷入。（两个包都刷入后再reboot） ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/3da8d7df73e63710ef1b44b0046a43a3.png) ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/4b099d299456e6917a435c167cd199d1.png) root(可选)：下载supersu卡刷包：[链接](https://www.miui.com/forum.php?mod=attachment&aid=OTMwNTQ3MXxjOTUxNDJjYnwxNTMzMjA1NDkwfDkwMDk3MjczN3w3MjYyOTMy "链接")，提前放入手机目录下，如上述方式一样卡刷进去后，重启手机。 ADB挂载（与卡刷二选一）： 由于博主wipe的时候，作死把Format data也顺手删除了一波，因此将手机目录中的rom全都清空了，不得已学习了一下ADB如何刷入rom包。原本觉得应该并不困难，没想到还遇到了意外的错误。 点击Advanced，点击ADB sideload。（手机与电脑需要连接，卡刷时不需要） ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/3da8d7df73e63710ef1b44b0046a43a3.png) ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/1b5cd96f28343a5c128be6da56afdb83.png) 打开ADB工具包所在文件夹，在空白处按下shift+鼠标右键，打开命令窗口。输入：adb devices，检测是否有设备接入。 ![](https://www.feiyuyu.xyz/wp-content/uploads/2018/08/1bbbeeed9230d8106deae1d685cfa96e.png) 连接成功后，输入：adb sideload ，并将电脑中所存在的rom压缩包（Aosp压缩包）拖入窗口，并回车。如果刷入成功自然很好，但是博主却遇到了一些超出我知识范围的问题，于是磨蹭了好久，终于找到了解决方案。 执行命令：adb nodaemon server ,查看adb server的端口是多少（一般是5037）。 再执行命令：netstat -ano|findstr "5037" ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/f9211f9c21cfb23eea74b43f06694dc1.png) 最后执行tskill 1576 ，也可打开任务管理器，右击名称栏，显示PID，结束掉对应PID的进程，重复adb sideload过程即可。 如果不会结束进程，不妨退出电脑上所有额外应用。

### 修复FC

已root的手机：[酷安下载](https://www.coolapk.com/apk/com.goplaycn.gappsrepair "酷安下载") 未root的手机：[酷安下载](https://www.coolapk.com/apk/top.gtf35.gappsfcrepair "酷安下载")，根据提示在adb窗口输入adb shell sh /sdcard/fix.sh 即可。

### 修复电信基带

只有电信版mi4需要进行此操作，提前将china-telcom-patch.zip放入手机目录中，卡刷入之。 接着开机，打开手机系统设置-移动网络-高级-首选网络类型-选择3G。 打开电话拨号盘，输入 ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/97cbe7edff48950cd0ca6ea11bf4f77c.png) ，进入手机信息，设置网络首选类型选择CDMA only，右上角选择无限装置频道，选择Cellular 800。此时应当会有电信的信号了。

### 修复原生系统中由于中国大陆无法直连Google导致的WiFi和信号标志上的x或!的问题

CaptiveMgr清除x和!：[酷安下载](https://www.coolapk.com/apk/tech.evlsoc.captivemgr "酷安下载") 最后由于Treble check检测到Rom不支持PT，我就放弃了继续刷入Android P，有兴趣的可继续参考[文章](https://blog.yuuta.moe/2018/07/19/flash-android-p/ "文章")。 #另附[云外科技（itxe）](https://www.itxe.net/ "云外科技（itxe）")所提供的一个查看电池损耗的应用（AccuBattery Pro）：[获取](https://www.itxe.net/usr/uploads/2018/07/366439055.apk "获取")