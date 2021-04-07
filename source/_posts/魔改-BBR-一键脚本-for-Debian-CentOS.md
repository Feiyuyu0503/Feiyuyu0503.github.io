---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: 魔改 BBR 一键脚本 for Debian & CentOS
tags:
  - 技术
excerpt: ''
date: 2018-03-30 15:09:55
---

 **BBR 是谷歌内部员工开发的新 TCP 拥塞控制技术，和锐速一样属于激进算法。其目的就是要尽量跑满带宽，并且尽量不要有排队的情况。** **此文所述版本，仅适用于 `KVM` 的 Debian 7+ (32/64 bit) 或 CentOS 6+ (64 bit)。**

开始使用
----

****只需 `运行脚本第一项+重启+运行脚本第二项`。一般用户只需使用此版本，并建议使用该版本。此版本不需要编译的过程，直接安装 v4.10.2 内核。****

    # Debian 7+
    # fool
    wget https://github.com/nanqinlang-tcp/tcp_nanqinlang/releases/download/3.4.2/tcp_nanqinlang-fool-1.2.2.shbash tcp_nanqinlang-fool-1.2.2.sh

  **这个是 `CentOS` 平台的版本，尚处于测试版，请勿在重要环境使用。**

    # CentOS 6/7
    # only 64 bit
    # devel
    wget https://raw.githubusercontent.com/nanqinlang-tcp/tcp_nanqinlang/master/General/CentOS/bash/tcp_nanqinlang-1.3.2.sh
    bash tcp_nanqinlang-1.3.2.sh

使用说明
----

**出现四个选项供以选择：**

### 安装内核

**用于安装内核`必须使用此命令安装内核并重启！`**

**确认内核更换完成后，`重启你的 vps`****重启开机后，`再次运行该脚本，选择第二项: 安装并开启算法`**

### **开启算法**

**用于编译并启用魔改 BBR 算法**

### 检查运行状态

**用于检查 tcp\_nanqinlang 是否已被 加载 (installed) 和 启用 (running)**

### 卸载

**不会删除已安装的内核，仅移除 sysctl.conf 中的相关设置项。然后重启机器后** **，魔改 BBR 才会停止运作。**

### 注意事项

1.  **一定要在执行完成 `安装内核` 并重启 vps 后，才能执行 `安装并启用算法`**
2.  **卸载命令不会改动内核**

**文章转自[南琴浪大佬](https://sometimesnaive.org)**
