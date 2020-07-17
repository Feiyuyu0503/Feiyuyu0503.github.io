---
title: 一只多功能的Telegram机器人
tags:
  - 技术
excerpt: ''
date: 2019-07-19 16:33:25
---

### 写在前面

一只能查快递，陪你聊天，查询美剧日剧等功能的Telegram机器人！成品可戳：[肥鱼机器](https://t.me/feiyuyubot "肥鱼机器")

**功能**

*   查快递
*   列出历史查询
*   聊天（包含语音、文字）
*   群组聊天（使用/开头、回复机器人消息来呼叫机器人）：由于开启了隐私模式，所以只好这样啦
*   美剧/日剧/电影查询

**功能预览** ![](http://www.feiyuyu.net/wp-content/uploads/2019/07/888.gif) ![](http://www.feiyuyu.net/wp-content/uploads/2019/07/889.gif) ![](http://www.feiyuyu.net/wp-content/uploads/2019/07/990.gif) ![](http://www.feiyuyu.net/wp-content/uploads/2019/07/991.gif)

### 搭建教程

Github：[项目地址](https://github.com/BennyThink/ExpressBot "项目地址") 1.申请一个Telegram bot 前往[BotFather](https://t.me/BotFather "BotFather"),点击`start`启用机器人之父，发送命令`/newbot`创建你自己的机器人，然后按照BotFather的提示，给机器人取名并输入消息`***bot`来自定义你的bot的用户名，然后就会生成属于你自己的机器人及其链接、token。继续发送命令`/mybots`,选择你的机器人然后选择`Edit bot`，选择`Edit Commands`，就可以在消息框下输入并发送如下指令：

    start         - 输入快递单号来查询
    query         - 查询美剧、电影
    yyets         - 查询下载链接
    weather       - 查询指定城市近期天气预报
    list          - 查看我的查询历史记录
    delete        - 删除某个单号查询记录
    quickdel      - 回复某条查询消息来快速删除单号查询记录
    help          - 帮助
    

2.部署 需要部署在可以访问 Telegram API 的服务器上，同时支持 Python 2和 Python 3，推荐用 Python 3 已经在以下平台测试通过： `Windows 10：Python 2.7.13 32bit Python 3.6.3 32bit Ubuntu 16.04/14.04、CentOS 7、Debian 9： Python 2.7` **部署方法1.自动脚本** 一键脚本在 systemd的情况下运行会更好，一键脚本仅测试于 Ubuntu 16.04： 先切换到 root 用户：

    wget -N --no-check-certificate https://raw.githubusercontent.com/BennyThink/ExpressBot/master/install.sh && bash install.sh
    

然后按照提示操作。支持 systemd 的系统会同时安装为 systemd 服务，其他系统可以使用对应的 init 手动配置或使用`supervisor` 快捷操作

    # 启动服务 
    bash install.sh start
    # 停止服务 
    bash install.sh stop
    

注：CentOS下如果提示`wget: command not found`请先安装wget:`yum install wget`

**部署方法2.手动配置** 如果一键脚本失败，可以试试手动配置 (1)克隆代码

    git clone https://github.com/BennyThink/ExpressBot
    cd ExpressBot
    

(2)准备环境 _Arch Linux_

    pacman -S python python-pip python-certifi python-chardet python-future python-idna python-requests python-six python-urllib3
    

然后从 AUR 安装 python-pytelegrambotapi _Ubuntu, Debian 等_

    sudo apt install python3 python3-pip git
    sudo pip3 install -r requirements.txt
    

_其他发行版、macOS_ Python3 请使用pip3替换pip

    pip install setuptools
    pip install -r requirements.txt
    

_Windows_ 从[Python官网](https://www.python.org/ "Python官网")下载并安装 Python，切换到项目目录，如果是 Python 2:

    pip install -r requirements.txt
    

如果是 Python 3，执行如下命令：

    pip3 install -r requirements.txt
    

(3)准备ffmpeg ffmpeg是为了支持音频识别（使用ffmpe进行音频文件的转码）。 如果你是 Windows ，从[这里](https://ffmpeg.org/ "这里")下载 ffmpeg 的二进制exe文件（一共三个都需要），放到 PATH 中； 如果你是 Linux 发行版，直接用包管理器安装就可以（编译或者下载二进制也行），Debian 系可以使用`sudo apt install ffmpeg`，RHEL可以使用`yum install ffmpeg` (4)配置 修改`config.py`进行配置，TOKEN 为 Bot 的 API，TURING\_KEY 若不配置则不启用机器人功能，TURING\_KEY在[这里](http://www.turingapi.com/ "这里")获取。

    TOKEN = 'Your TOKEN'
    TURING_KEY = 'Your Key'
    

创建单元文件：`vim /lib/systemd/system/expressbot.service` 自行替换输入如下信息

    [Unit]  
    Description=A Telegram Bot for querying expresses   
    After=network.target network-online.target nss-lookup.target    
    
    [Service]   
    Restart=on-failure  
    Type=simple 
    ExecStart=/usr/bin/python /home/ExpressBot/expressbot/main.py   
    
    [Install]   
    WantedBy=multi-user.target
    

重新载入 daemon、自启、启动

    systemctl daemon-reload
    systemctl enable expressbot.service
    systemctl start expressbot.service
    

使用了`restart=on-failure`参数，失败退出会重启。 如果设置成always就意味着无论因为什么原因，只要进程不在了，systemd 就会立刻帮我们重启。详情可以参见systemd.service手册。 (5)运行 测试目的的话，以 nohub 或 screen 运行`main.py`，Python 3 请用python3替换为python

    cd /home/ExpressBot/expressbot
    nohup python main.py
    # 或者
    cd /ExpressBot/expressbot
    screen -S tgbot
    python main.py
    

(6)计划任务 如果需要追踪更新并推送，那么咱需要定期轮询。 目前使用的定时器是 apscheduler，`config.py`中的 `INTERVAL`可以来设置间隔时间 (7)检查运行状态 systemd 控制命令：

    # 查看运行状态
    sudo systemctl status expressbot.service
    # 启动
    sudo systemctl start expressbot.service
    # 停止
    sudo systemctl stop expressbot.service
    # 重启
    sudo systemctl restart expressbot.service
    

其他系统 可以考虑使用对应系统的init，或者使用supervisor

**部署方法3.使用 Docker** 目前支持 docker 运行，但是尚未经过详细测试。 (1)拉取镜像

    docker pull bennythink/expressbot:latest
    

(2)后台运行

    docker run -d --restart=always -e TOKEN="TOKEN" -e TURING="KEY"  expressbot:v1
    

如果想自己 build 的话，那么就下载回 Dockerfile，然后

    docker build -t expressbot:v1 .
    

* * *

更多详细内容请查看[项目地址](https://github.com/BennyThink/ExpressBot "项目地址")