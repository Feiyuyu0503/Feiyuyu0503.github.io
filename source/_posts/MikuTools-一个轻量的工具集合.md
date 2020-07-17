---
title: MikuTools-一个轻量的工具集合
tags:
  - 技术
excerpt: ''
date: 2019-06-27 00:12:34
---

### 写在前面

MikuTools是一款开源的轻量的小工具合集，不过只包括了部分无需后端的工具。官方demo：[](https://miku.tools "https://miku.tools")[https://miku.tools](https://miku.tools) Github：[](https://github.com/Ice-Hazymoon/MikuTools "https://github.com/Ice-Hazymoon/MikuTools")[https://github.com/Ice-Hazymoon/MikuTools](https://github.com/Ice-Hazymoon/MikuTools) 自建demo：[](https://tools.feiyuyu.net "https://tools.feiyuyu.net")[https://tools.feiyuyu.net](https://tools.feiyuyu.net)

### 搭建教程

1、安装Nodejs

    #Debian/Ubuntu系统
    curl -sL https://deb.nodesource.com/setup_10.x | bash -
    apt install -y nodejs git 
    
    #CentOS系统
    curl -sL https://rpm.nodesource.com/setup_10.x | bash -
    yum install nodejs git -y
    

2、安装yarn

    npm install -g yarn
    

3、拉取项目

    #拉取项目
    git clone https://github.com/Ice-Hazymoon/MikuTools
    #进入文件夹
    cd MikuTools
    #安装依赖
    yarn
    

4、构建

    #开发模式运行，不知道有啥用的，可以跳过，直接构建
    yarn dev
    
    #构建
    yarn generate
    

此时打包后的源码在**dist**文件夹，然后就可以将文件夹里的源码丢到自己的网站根目录直接用了。

如果你想提前看下效果，那这里提供一个最快的运行方法，使用命令：

    #进入打包好的文件夹
    cd dist
    #运行端口4567，可自行修改，提前检查端口开放否
    python -m SimpleHTTPServer 4567
    

5、使用Caddy 如果你服务器没有任何Web环境，比如Nginx、Apache等，那可以直接使用Caddy部署一个网站。 安装Caddy：

    wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh
    #备用地址
    wget -N --no-check-certificate https://www.moerats.com/usr/shell/Caddy/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh
    

配置Caddy：

    #请修改域名和dist文件夹绝对路径后一起复制到SSH运行！
    
    #http访问，该配置不会自动签发SSL
    echo "http://tools.feiyuyu.net:80 {
     root /root/MikuTools/dist
     gzip
    }" > /usr/local/caddy/Caddyfile
    
    #https访问，该配置会自动签发SSL，请提前解析域名到VPS服务器
    echo "https://tools.feiyuyu.net:443 {
     root /root/MikuTools/dist
     tls seuff@feiyuyu.net
     gzip
    }" > /usr/local/caddy/Caddyfile
    

tls参数会自动帮你签发ssl证书，如果你要使用自己的ssl，改为`tls /root/xx.crt /root/xx.key`即可。后面为ssl证书路径。

启动Caddy：

    /etc/init.d/caddy start
    

这样就可以打开域名进行访问了。

* * *

转自：[](https://www.moerats.com/archives/945/ "https://www.moerats.com/archives/945/")[https://www.moerats.com/archives/945/](https://www.moerats.com/archives/945/)