---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: Debian/Ubuntu/CentOS 网络安装/重装系统/纯净安装 一键脚本
tags:
  - 技术
excerpt: ''
date: 2018-08-14 17:51:14
---

### 写在前面

全自动安装默认root密码:Vicer,安装完成后请立即更改密码 全自动安装CentOS时默认提供VNC功能,可使用VNC Viewer查看进度,VNC端口为1或者5901,可自行尝试连接。(成功后VNC功能会消失。) 目前CentOS系统只支持任意版本重装为 CentOS 6.x 及以下版本。 萌咖提供的dd安装镜像，远程登陆账号为: Administrator，远程登陆密码为: Vicer。 OpenVZ架构不适用。

### 依赖包:

    #二进制文件    Debian/Ubuntu    RedHat/CentOS
    iconv         [libc-bin]       [glibc-common]
    xz            [xz-utils]       [xz]
    awk           [gawk]           [gawk]
    sed           [sed]            [sed]
    file          [file]           [file]
    grep          [grep]           [grep]
    openssl       [openssl]        [openssl]
    cpio          [cpio]           [cpio]
    gzip          [gzip]           [gzip]
    cat,cut..     [coreutils]      [coreutils]
    

### 确保安装了所需软件:

    #Debian/Ubuntu:
    apt-get install -y xz-utils openssl gawk file
    
    #RedHat/CentOS:
    yum install -y xz openssl gawk file
    如果出现了错误,请运行:
    

### 如果出现了错误,请运行:

    #Debian/Ubuntu:
    apt-get update
    
    #RedHat/CentOS:
    yum update
    

### 一键下载及使用:

    wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && chmod a+x InstallNET.sh
    

    Usage:
            bash InstallNET.sh      -d/--debian [dist-name]
                                    -u/--ubuntu [dist-name]
                                    -c/--centos [dist-version]
                                    -v/--ver [32/i386|64/amd64]
                                    --ip-addr/--ip-gate/--ip-mask
                                    -apt/-yum/--mirror
                                    -dd/--image
                                    -a/-m
    
    # dist-name: 发行版本代号
    # dist-version: 发行版本号
    # -apt/-yum/--mirror : 使用定义镜像
    # -a/-m : 询问是否能进入VNC自行操作. -a 为不提示(一般用于全自动安装), -m 为提示.
    
    

### 使用示例:

    #使用默认镜像全自动安装
    bash InstallNET.sh -d 8 -v 64 -a
    
    #使用自定义镜像全自动安装
    bash InstallNET.sh -c 6.9 -v 64 -a --mirror 'http://mirror.centos.org/centos'
    
    
    # 以下示例中,将X.X.X.X替换为自己的网络参数.
    # --ip-addr :IP Address/IP地址
    # --ip-gate :Gateway   /网关
    # --ip-mask :Netmask   /子网掩码
    
    #使用自定义镜像自定义网络参数全自动安装
    #bash InstallNET.sh -u 16.04 -v 64 -a --ip-addr x.x.x.x --ip-gate x.x.x.x --ip-mask x.x.x.x --mirror 'http://archive.ubuntu.com/ubuntu'
    
    #使用自定义网络参数全自动dd方式安装
    #bash InstallNET.sh --ip-addr x.x.x.x --ip-gate x.x.x.x --ip-mask x.x.x.x -dd 'https://moeclub.org/get-win7embx86-auto'
    
    #使用自定义网络参数全自动dd方式安装存储在谷歌网盘中的镜像(调用文件ID的方式)
    #bash InstallNET.sh --ip-addr x.x.x.x --ip-gate x.x.x.x --ip-mask x.x.x.x -dd "$(echo "14xB2k9r36NRzWhO0lzjf604EB7B6JLH0" |xargs -n1 bash <(wget --no-check-certificate -qO- 'https://moeclub.org/get-gdlink'))"
    
    #使用自定义网络参数全自动dd方式安装存储在谷歌网盘中的镜像
    #bash InstallNET.sh --ip-addr x.x.x.x --ip-gate x.x.x.x --ip-mask x.x.x.x -dd "$(echo "https://drive.google.com/open?id=14xB2k9r36NRzWhO0lzjf604EB7B6JLH0" |xargs -n1 bash <(wget --no-check-certificate -qO- 'https://moeclub.org/get-gdlink'))"
    

### 特别提示

在dd安装系统镜像时: 在你的机器上全新安装,如果你有VNC,可以看到全部过程. 在dd安装镜像的过程中,不会走进度条(进度条一直显示为0%).完成后将会自动重启. 分区界面标题一般显示为: “Starting up the partitioner“ 使用谷歌网盘中储存的镜像: [\[无限制大小\] 获取谷歌网盘文件临时直接下载链接](https://moeclub.org/2018/04/01/600/?v=425 "[无限制大小] 获取谷歌网盘文件临时直接下载链接") 在全自动安装CentOS时: 如果看到 “Starting graphical installation” 或者类似表达,则表示正在安装. 正常情况下只需要耐心等待安装完成即可. 如果需要查看进度,使用VNC Viewer(或者其他VNC连接工具) 连接提示中的IP地址:端口进行连接.(端口一般为1或者5901)

* * *

* * *

原文地址:[获取](https://moeclub.org/2018/04/03/603/)

感谢萌咖大佬的脚本。
