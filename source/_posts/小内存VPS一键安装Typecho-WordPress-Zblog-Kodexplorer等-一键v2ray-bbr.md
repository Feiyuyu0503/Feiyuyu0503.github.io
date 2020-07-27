---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 小内存VPS一键安装Typecho/WordPress/Zblog/Kodexplorer等+一键v2ray+bbr
tags:
  - 技术
excerpt: ''
date: 2018-10-05 19:09:21
---

Github：[前往](https://github.com/dylanbai8/Onekey_Caddy_PHP7_Sqlite3 "前往")

### 脚本特性

*   小内存VPS 一键安装 Caddy+PHP7+Sqlite3 环境 （支持VPS最小内存64M）
*   一键绑定域名自动生成SSL证书开启https（ssl自动续期）、支持IPv6
*   一键安装 typecho、wordpress、zblog、kodexplorer、laverna、一键整站备份
*   一键安装 v2ray、rinetdbbr
*   经典组合 \[Website(caddy+php7+sqlite3+tls)+V2ray(vmess+websocket)\]use\_path+Rinetdbbr
*   支持系统：Centos 7+ Debian 8+ （建议选择 Debian 8 mini版）

### 安装

**建议使用前提前解析好域名，否则签发SSL会失败。** **支持IPv6（AAAA记录）如果本地网络不支持IPv6可以通过cloudflareCDN转换为IP4** 执行命令： `wget -N --no-check-certificate git.io/c.sh && chmod +x c.sh && bash c.sh` 此时会安装好Caddy、PHP7、Sqlite3环境。 安装好环境后，再使用以下命令安装所需要的程序。

    #一键安装typecho博客
    bash c.sh -t
    
    #一键安装wordpress博客
    bash c.sh -w
    
    #一键安装zblog博客
    bash c.sh -z
    
    #一键安装kodexplorer可道云
    bash c.sh -k
    
    #一键安装laverna印象笔记
    bash c.sh -l
    
    #一键整站备份（一键打包/www网站目录、含数据库）
    bash c.sh -a
    
    #一键安装v2ray
    bash c.sh -v
    
    #一键安装rinetd bbr端口加速
    bash c.sh -b
    

### 卸载

    卸载 caddy
    bash c.sh -unc
    
    卸载 php+sqlite
    bash c.sh -unp
    
    卸载 v2ray
    bash c.sh -unv
    
    卸载 rinetdbbr
    bash c.sh -unb
    

### 其他说明

启动：`/etc/init.d/caddy start` 停止：`/etc/init.d/caddy stop` 重启：`/etc/init.d/caddy restart`

查看状态：`/etc/init.d/caddy status` 查看Caddy日志：`tail -f /tmp/caddy.log`

Caddy安装目录：`/usr/local/caddy` Caddy配置文件位置：`/usr/local/caddy/Caddyfile`

网站目录：`/www`
