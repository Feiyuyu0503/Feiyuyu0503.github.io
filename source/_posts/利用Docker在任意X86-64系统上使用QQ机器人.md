---
title: 利用Docker在任意X86_64系统上使用QQ机器人
tags:
  - 日志
  - 技术
excerpt: ''
date: 2018-07-10 13:33:58
---

### 使用方法

确保已安装部署好Docker（[安装请看这](http://www.runoob.com/docker/docker-tutorial.html "安装请看这。")）安装docker后，键入以下命令下载 酷Q Docker 镜像：

    docker pull coolq/wine-coolq
    

下载后，在任意目录创建一个空文件夹，用于持久化存放 酷Q 数据：

    mkdir /root/coolq-data # 任意路径均可
    

最后运行 酷Q 镜像：

    docker run --name=coolq --rm -p 9000:9000 -v /root/coolq-data:/home/user/coolq -e VNC_PASSWD=12345678 -e COOLQ_ACCOUNT=233333 coolq/wine-coolq
    

参数含义

参数事例

远程监听端口

9000

数据存放位置

/root/coolq-data

远程访问密码

12345678

机器人QQ帐号

233333

运行后，会输出一系列日志。当看到 \[CQDaemon\] Started CoolQ . 时，说明已启动成功。 此时，在浏览器中访问 服务器IP:监听端口 即可看到远程操作登录页面。 在登录后，右键点击悬浮窗 -> 你的 QQ 昵称 -> 勾选「自动登录」，即可保证 酷Q 能自动登录。 应用管理中开启插件功能，图灵机器人需要去[官网](http://www.tuling123.com "官网")申请自己的API

### 管理命令

`docker run --name=coolq -d -p 8080:8080 -v /root/coolq-data:/home/user/coolq -e VNC_PASSWD=12345678 -e COOLQ_ACCOUNT=233333 coolq/wine-coolq` #启动容器（只需将--rm替换为-d） `docker logs coolq` #查看运行状况 `docker start coolq` #启动服务 `docker stop coolq` #停止服务 更多内容请查看[此处](https://github.com/CoolQ/docker-wine-coolq "此处")