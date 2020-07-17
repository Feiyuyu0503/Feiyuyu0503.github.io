---
title: PyOne-又一款OneDrive目录浏览系统
tags:
  - 技术
excerpt: ''
date: 2019-02-01 08:15:27
---

### 写在前面

PyOne是一款基于Python-Flask的onedrive文件本地化浏览系统，使用MongoDB储存文件列表，使用redis缓存数据，支持绑定多个onedrive网盘且支持离线下载，极大的提高使用效率。

### 预览

demo：[](https://pyone.feiyuyu.net "https://pyone.feiyuyu.net")[https://pyone.feiyuyu.net](https://pyone.feiyuyu.net)

![20190131201440.png](https://i.loli.net/2019/01/31/5c52e6deb72c4.png) ![20190131201621.png](https://i.loli.net/2019/01/31/5c52e726a080e.png)

### 特性

简单易用
====

只需简单设置，即可做一个onedrive文件列表分享程序。

功能丰富
====

可设置文件夹密码。只需在文件夹添加.password文件，内容为密码内容，即可在该文件夹设置密码 可设置README。

后台强大
====

防盗链设置。 后台上传文件。 后台更新文件。 后台设置统计代码 后台管理onedrive文件。 直接删除onedrive文件 直接在后台给文件夹添加.password和README和HEAD 直接在后台编辑文本文件。 上传本地文件至onedrive 支持创建文件夹 支持移动文件（仅限单文件）

支持绑定多网盘
=======

支持离线下载
======

### 安装

Github地址：[](https://github.com/abbeyokgo/PyOne "https://github.com/abbeyokgo/PyOne")[https://github.com/abbeyokgo/PyOne](https://github.com/abbeyokgo/PyOne) 1.安装宝塔

    #Centos系统
    yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
    #Ubuntu系统
    wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh
    #Debian系统
    wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && bash install.sh
    

然后到**软件管理-运行环境-选择安装Nginx、MongoDB和Redis**。（Nginx安装时间较长）

![QQ截图20190131160944.png](https://i.loli.net/2019/01/31/5c52e8d48b8ae.png)

2.下载源码

    git clone https://github.com/abbeyokgo/PyOne.git
    

3、安装依赖

    cd /root/PyOne
    pip install -r requirements.txt
    

4.运行

    #复制配置文件
    cp config.py.sample config.py
    cp supervisord.conf.sample supervisord.conf
    #运行
    gunicorn -w4 -b 0.0.0.0:34567 run:app
    

然后试着访问：[http://ip:34567](http://ip:34567) 看看能正常显示，如果不能，请在宝塔的安全里开启端口。 5.安装Aria2

    git clone https://github.com/abbeyokgo/aria2_installer.git
    cd aria2_installer
    sh install_aria2.sh
    

6.绑定域名 先确保域名已经解析到你的服务器ip 打开宝塔-网站-添加站点 设置反代：宝塔-网站-点击域名-反向代理，设置值http://127.0.0.1:34567 然后勾选启用反向代理 添加nginx配置：宝塔-网站-点击域名-配置文件。找到以下内容，添加以下三行。

    location / 
        {
            ...
    
            proxy_buffering off;
            proxy_cache off;
            proxy_set_header X-Forwarded-Proto $scheme;
    
            ...
        }
    

![snipaste_2018-11-19_10-45-18.png](https://i.loli.net/2019/01/31/5c52ebf44d6f2.png)

做完以上操作，应该就可以访问你的域名了！

7.开机启动 网站源码下有个**supervisord.conf**，主要内容如下： `[program:pyone] command = gunicorn -k eventlet -b 0.0.0.0:34567 run:app directory = /root/PyOne autorestart = true` 主要修改两个地方： 端口号：即34567那个端口号，修改为自己选的，或者不改动 源码目录：directory修改为你选的网站目录 修改之后运行下面的命令（记得修改为正确的目录），设置开机启动：

    echo "supervisord -c /root/PyOne/supervisord.conf" >> /etc/rc.d/rc.local
    chmod +x /etc/rc.d/rc.local
    supervisord -c /root/PyOne/supervisord.conf
    

### 常见问题

文档：[错误指导](https://wiki.pyone.me/cuo-wu-zhi-dao/ "错误指导")