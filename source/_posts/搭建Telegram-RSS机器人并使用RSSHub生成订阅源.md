---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: 搭建Telegram RSS机器人并使用RSSHub生成订阅源
tags:
  - 技术
excerpt: ''
date: 2019-05-04 01:51:42
---

### 写在前面

经常逛论坛或者浏览各类咨询网站的人可能会使用到RSS，RSS（简易信息聚合）是一种消息来源格式规范，用以聚合经常发布更新数据的网站，例如博客文章、新闻、音频或视频的网摘。话不多说了，对于Telegram重度使用者而言，用Telegram bot来实现RSS阅读，真是太方便了。 搭建之前，先推荐两个能发现当前网页RSS的扩展插件 [RSS+ : 显示当前网站所有的 RSS](https://greasyfork.org/zh-CN/scripts/373252-rss-show-site-all-rss "RSS+ : 显示当前网站所有的 RSS")：可以发现 RSS 的 Tampermonkey 脚本(效果不错，很美观，只是目前发现对于RSSHub所生成的B站Feed规则有些不对) [Easy-to-RSS](https://github.com/idealclover/Easy-to-RSS "Easy-to-RSS")：貌似是友校NJU的大佬做的，与RSSHub联动，可以将RSSHub的服务网址换成自己的。

### 搭建教程

Github项目地址：[](https://github.com/iovxw/rssbot "https://github.com/iovxw/rssbot")[https://github.com/iovxw/rssbot](https://github.com/iovxw/rssbot) **1.申请一个Telegram bot。** 前往[BotFather](https://t.me/BotFather "BotFather"),点击`start`启用机器人之父，发送命令`/newbot`创建你自己的机器人，然后按照BotFather的提示，给机器人取名并输入消息`***bot`来自定义你的bot的用户名，然后就会生成属于你自己的机器人及其链接、token。继续发送命令`/mybots`,选择你的机器人然后选择`Edit bot`，选择`Edit Commands`，就可以在消息框下输入并发送如下指令：

    /rss       - 显示当前订阅的 RSS 列表，加 raw 参数显示链接
    /sub       - 订阅一个 RSS: /sub http://example.com/feed.xml
    /unsub     - 退订一个 RSS: /unsub http://example.com/feed.xml
    /unsubthis - 使用此命令回复想要退订的 RSS 消息即可退订, 不支持 Channel
    /export    - 导出为 OPML
    

**2.服务器部署机器人** 2.1 安装相关依赖环境

    #CentOS系统
    yum -y update && yum -y install gcc make openssl* pkg* libssl* screen curl
    
    #Ubuntu、Debian系统
    apt-get -y update && apt-get -y install gcc make openssl pkg-config libssl-dev screen curl
    

2.2 安装Rust Nightly

    curl https://sh.rustup.rs -sSf | sh
    source $HOME/.cargo/env
    

2.3 安装rssbot

    wget https://github.com/iovxw/rssbot/archive/v1.4.4.tar.gz
    tar xvf v1.4.4.tar.gz
    cd rssbot-1.4.4
    cargo build --release
    

2.4 在screen下运行rssbot

    cd target/release
    screen -S rssbot
    ./rssbot DATAFILE TELEGRAM-BOT-TOKEN
    

`DATAFILE`为数据库保存路径(其实就是一个json文件，不需要手动创建)。`TELEGRAM-BOT-TOKEN`就是你创建的机器人的token。 搭建成功后，就可以对bot发送命令，来订阅你想要的RSS了。

### 部署RSSHub

其实在上面一部分，我们的工作已经完成了。但对于没有Feed的网站而言，我们是不能对其生成RSS订阅的。这时，一个号称`万物皆可RSS`的项目`RSSHub`，就诞生了。 RSSHub 是一个轻量、易于扩展的 RSS 生成器, 可以给任何奇奇怪怪的内容生成 RSS 订阅源。[前往文档查看详情](https://docs.rsshub.app/ "前往文档查看详情") RSSHub的部署很简单，这里我以Centos7系统为例，使用Docker部署，你也可以查看[官方部署文档](https://docs.rsshub.app/install/ "官方部署文档") 1.安装 Docker

    curl -sSL https://get.docker.com/ | sh
    systemctl start docker
    systemctl enable docker
    

2.拉取RSSHub镜像并启动

    docker pull diygod/rsshub
    docker run -d --name rsshub -p 1200:1200 diygod/rsshub
    

至此，只需浏览器访问 `ip:1200`，就可以看见RSSHub已经在运行了。 3.应用配置 配置运行在 docker 中的 RSSHub, 最便利的方法是使用 docker 环境变量 以设置缓存时间为 1 小时举例, 只需要在运行时增加参数:`-e CACHE_EXPIRE=3600`

    $ docker run -d --name rsshub -p 1200:1200 -e CACHE_EXPIRE=3600 -e GITHUB_ACCESS_TOKEN=example diygod/rsshub
    

更多相关配置请查阅[官方文档](https://docs.rsshub.app/install/#%E9%85%8D%E7%BD%AE "官方文档") 另外，对于一个已启动的Docker容器，如果我们需要添加运行参数，比如订阅Pixiv的feed时，要求我们添加P站登陆账号的参数，可以使用如下命令：

    docker ps       #查看当前运行Docker容器详情
    docker container update -e PIXIV_USERNAME=xxxxx -e PIXIV_PASSWORD=xxxx  <containername> 
    

关闭RSSHub时，可使用命令：

    docker stop rsshub
    

如果你没有自己的服务器，当然也可以使用官方提供的[rsshub.app](https://rsshub.app "rsshub.app"),为了减轻服务器压力，更推荐自建服务，也可以使用我的自建RSSHub服务和Telegram bot：[](https://rss.feiyuyu.net "https://rss.feiyuyu.net")[https://rss.feiyuyu.net](https://rss.feiyuyu.net)和[feiyurss\_bot](https://t.me/feiyurss_bot "feiyurss_bot") (能用多久是个缘，我尽量维护

### 配合IFTTT的使用

IFTTT，是一个新生的网络服务平台，通过其他不同平台的条件来决定是否执行下一条命令。即对网络服务通过其他网络服务作出反应。IFTTT得名为其口号“if this then that”。 官网：[](https://ifttt.com/discover "https://ifttt.com/discover")[https://ifttt.com/discover](https://ifttt.com/discover) 在上面，我们已经讲了如何部署Telegram bot来推送RSS订阅。这部分扩展一下，我们也可以通过添加IFTTT的官方TG bot：[IFTTT](https://t.me/IFTTT "IFTTT"),搭配两个官网上的小程序（Applets）：[RSS Feed](https://ifttt.com/feed "RSS Feed")和[telegram](https://ifttt.com/telegram "telegram")，实现RSS的推送。 你可以在tg端与IFTTT私聊或将IFTTT添加到channel或group，然后在IFTTT网站或app上[添加你的Applets](https://ifttt.com/create "添加你的Applets")，点击`+this`，搜索RSS Feed，然后添加你想要的rss feed的url，确认后网页或app会回到“if this then that”，点击`+that`，搜索telegram，选择Target chat，以及自定义化Message text，最后save你的Applets并给其命名，当订阅更新时，tg端的IFTTT会自动给你选择的Target chat发推送。当然，得益于IFTTT的强大，我们也可以不用到RSS Feed这个Applets，你可以使用其Youtube、Twitter的Applets，设定Trigger，再推送到tg。 示例：![img](https://i.loli.net/2020/03/11/B9WtbH1A4Eum3V7.jpg) 截图真的丑（逃。 另外，强烈建议在[](https://ifttt.com/settings "https://ifttt.com/settings")[https://ifttt.com/settings](https://ifttt.com/settings)的设定中关闭URL shortening，因为bit.ly这个短链接服务，在国内体验不是很好。（光速逃

* * *

最后欢迎订阅我的Telegram channel：[肥鱼Blog](https://t.me/feiyuyu "肥鱼Blog") 或者RSS channel：[肥鱼的Rss推送](https://t.me/rss_feiyuyu "肥鱼的Rss推送")，只有我的复读，没有女装。
