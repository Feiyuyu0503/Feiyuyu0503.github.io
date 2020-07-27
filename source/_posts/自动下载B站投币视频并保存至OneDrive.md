---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 自动下载B站投币视频并保存至OneDrive
tags:
  - 技术
excerpt: ''
date: 2019-10-06 15:51:00
---

### 写在前面

因为手上的OneDrive 5T账号实在太多了，不用实属可惜，于是就想存点视频到OneDrive里吧。每次都手动下载再上传到网盘又太麻烦了，那么有没有什么适合懒人的办法呢？果然网上还是有大佬给出了方案。以B站投币视频为例，想要实现将投币的视频自动保存到OneDrive里，配合[pyone](http://www.feiyuyu.net/archives/1394 "pyone")/onelist等程序也能在线播放岂不是美滋滋，也可以不必担心自己的B站收藏夹满是失效视频了，不是吗 {{tiaokan}} **方案：投币操作 -> RSS 更新 -> IFTTT 触发 Webhook -> 服务器下载**

### 自动流程

**1.给视频投币**![](http://www.feiyuyu.net/wp-content/uploads/2019/10/3b0608e520bc03dc1306e1de2f842b8c.png) **2.RSS更新**(搭配[RSSHub](http://www.feiyuyu.net/archives/1475 "RSSHub")订阅B站投币Feed) ![](http://www.feiyuyu.net/wp-content/uploads/2019/10/2722cd2cad3b99e8b1c52d147fbd1e72.png) **3.IFTTT 触发 webhook** ![](http://www.feiyuyu.net/wp-content/uploads/2019/10/52fbf52dcbc102230829f81f3a2e6b13.png) **4.webhook收到下载请求** 下载B站视频很简单，you-get 一行命令的事。 ![](http://www.feiyuyu.net/wp-content/uploads/2019/10/035be1391914368f6eeebcbfa7b6e32a.png) **5.完成下载** ![](http://www.feiyuyu.net/wp-content/uploads/2019/10/e8b0e99c8a175b76dcc394eddb3b4dbe.png) **6.rclone定时执行任务** 利用rclone将服务器上下载得到的视频定时上传至OneDrive网盘

### 搭建教程

**以下均已CentOS 7系统为例。** 先决条件： [Python](https://www.python.org/downloads/ "Python") 3.2 or above [FFmpeg](https://www.ffmpeg.org/ "FFmpeg") 1.0 or above (Optional) [RTMPDump](https://rtmpdump.mplayerhq.hu/ "RTMPDump") **1.安装you-get**(更多安装方式可查看:[Github](https://github.com/soimort/you-get/wiki/%E4%B8%AD%E6%96%87%E8%AF%B4%E6%98%8E "Github"))

    pip3 install you-get
    

**2.安装Nodejs**

    curl -sL https://rpm.nodesource.com/setup_10.x | bash -
    yum install nodejs git -y
    

**3.部署[DIYgod](https://diygod.me/ "DIYgod")大佬写的[download-webhook](https://github.com/DIYgod/download-webhook "download-webhook")**

    git clone https://github.com/DIYgod/download-webhook.git
    yum -y install screen
    cd download-webhook
    npm install
    screen -S webhook    #screen中运行
    npm run start        #运行后使用Ctrl+A+D隐藏screen
    
    

此时 [http://服务器ip:3000](http://服务器ip:3000) 即为webhook的url 尝试命令

    curl -X POST -H "Content-Type:application/json" -d '{"secret": "mysecret", "path": "mypath", "name": "myvideo", "url": "https://www.bilibili.com/video/av66129247"}' http://127.0.0.1:3000
    

如果下载成功会在服务器目录下生成一个mypath的文件夹，里面存放着叫myvideo的视频。 然后在ifttt中设定applet，内容如上自动流程中的第3步。 也可使用命令`screen -r webhook`查看下载详情 **4.安装rclone**

    yum install fuse
    curl https://rclone.org/install.sh | sudo bash
    rclone config
    
    

接着出现以下内容

    Current remotes:
    
    Name                 Type
    ====                 ====
    craftcloud           onedrive
    
    e) Edit existing remote
    n) New remote
    d) Delete remote
    r) Rename remote
    c) Copy remote
    s) Set configuration password
    q) Quit config
    e/n/d/r/c/s/q> n     #新建与网盘的连接
    name> example        #自定义的网盘名称，如我已经连接过的craftcloud
    Type of storage to configure.
    Enter a string value. Press Enter for the default ("").
    Choose a number from below, or type in your own value
     1 / 1Fichier
       \ "fichier"
     2 / Alias for an existing remote
       \ "alias"
     3 / Amazon Drive
       \ "amazon cloud drive"
     4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
       \ "s3"
     5 / Backblaze B2
       \ "b2"
     6 / Box
       \ "box"
     7 / Cache a remote
       \ "cache"
     8 / Dropbox
       \ "dropbox"
     9 / Encrypt/Decrypt a remote
       \ "crypt"
    10 / FTP Connection
       \ "ftp"
    11 / Google Cloud Storage (this is not Google Drive)
       \ "google cloud storage"
    12 / Google Drive
       \ "drive"
    13 / Google Photos
       \ "google photos"
    14 / Hubic
       \ "hubic"
    15 / JottaCloud
       \ "jottacloud"
    16 / Koofr
       \ "koofr"
    17 / Local Disk
       \ "local"
    18 / Mega
       \ "mega"
    19 / Microsoft Azure Blob Storage
       \ "azureblob"
    20 / Microsoft OneDrive
       \ "onedrive"
    21 / OpenDrive
       \ "opendrive"
    22 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
       \ "swift"
    23 / Pcloud
       \ "pcloud"
    24 / Put.io
       \ "putio"
    25 / QingCloud Object Storage
       \ "qingstor"
    26 / SSH/SFTP Connection
       \ "sftp"
    27 / Union merges the contents of several remotes
       \ "union"
    28 / Webdav
       \ "webdav"
    29 / Yandex Disk
       \ "yandex"
    30 / http Connection
       \ "http"
    31 / premiumize.me
       \ "premiumizeme"
    Storage> 20      #选择20 OneDrive，Google Drive等同理
    ** See help for onedrive backend at: https://rclone.org/onedrive/ **
    
    Microsoft App Client Id
    Leave blank normally.
    Enter a string value. Press Enter for the default ("").
    client_id>        #留空
    Microsoft App Client Secret
    Leave blank normally.
    Enter a string value. Press Enter for the default ("").
    client_secret>    #留空
    Edit advanced config? (y/n)
    y) Yes
    n) No
    y/n> n
    Remote config
    Use auto config?
     * Say Y if not sure
     * Say N if you are working on a remote or headless machine
    y) Yes
    n) No
    y/n> n            #因为是服务器，所以选择n
    For this to work, you will need rclone available on a machine that has a web browser available.
    Execute the following on your machine (same rclone version recommended) :
        rclone authorize "onedrive"
    Then paste the result below:
    result>  {"access_token":""}    #输入在客户端授权的内容(下一步将讲如何获取)
    [example]
    client_id = 
    client_secret = 
    token = {"access_token":""}
    
    y) Yes this is OK
    e) Edit this remote
    d) Delete this remote
    y/e/d> y  选择y
    Current remotes:
    
    Name                 Type
    ====                 ====
    craftcloud           onedrive
    example              onedrive
    
    e) Edit existing remote
    n) New remote
    d) Delete remote
    r) Rename remote
    c) Copy remote
    s) Set configuration password
    q) Quit config
    e/n/d/r/c/s/q> q  #选择q退出
    

客户端授权 在本地Windows电脑上下载rclone，下载地址[https://rclone.org/downloads/](https://rclone.org/downloads/) 然后解压出来，比如我解压到D盘，文件夹命名rclone，此时点击Win+R，然后输入cmd，确定。再输入以下命令：

    d:
    cd rclone
    rclone authorize "onedrive"
    
    #此后会出现以下信息
    D:\rclone>rclone authorize "onedrive"
    If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
    Log in and authorize rclone for access
    Waiting for code...      #弹出浏览器，选择onedrive账号登陆进行授权
    Got code
    Paste the following into your remote machine --->
    {"access_token"j61aF6QzwiUX7nxesr9MMK68kcAgBWU0na0thal0EcVCBmGQHXNsTlgXuhP1Y4ln5bAC7o1ZUltCCijs-wyEM-SU2P77fRCgEoSeJEkIAA","expiry":"2019-10-06T15:58:55.8424381+08:00"}
    #复制{"access_token"xxx}整个内容，粘贴至上一步中 result> 处
    <---End paste
    D:\rclone>
    

**5.挂载OneDrive为服务器磁盘** （也可不挂载，使用rclone sync/rclone copy等命令完成下载内容上传至网盘的操作，具体可查看rclone使用方法，这里我们挂载为磁盘）

    #新建本地文件夹，路径自己定，即下面的LocalFolder
    mkdir /root/OneDrive
    #挂载为磁盘
    rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
    #DriveName为初始化配置填的name，Folder为OneDrive里的文件夹，LocalFolder为VPS上的本地文件夹。
    
    

**6.定时上传网盘** 我希望每天下载到vps的视频都自动上传至OneDrive，然后上传完后就不占用vps的磁盘空间，所以每天定时执行 mv命令。 配合 crontab 定时执行，比如每天 2:33 执行一次，日志写到 /root/rclone.log。执行 `crontab -e`，按i编辑

    33 2 * * * /usr/bin/mv /root/download-webhook/downloads/* /root/OneDrive >> /root/rclone.log 2>&1
    #其中/root/download-webhook/downloads为下载到vps的本地文件夹， /root/OneDrive 为挂载到vps的网盘空间
    

按下ESC，输入 :wq 保存并退出。 重启下crond服务 `service crond restart`

### 更多（复读）

以上流程同样适用于自动下载 YouTube \\ Instagram \\ Tumblr 视频、网易云音乐歌曲等，只要 RSSHub 和 you-get 支持即可。 另外对于图片，Webhook URL 参数直接传入图片地址也可以下载，所以也可以轻松实现自动下载 Bing 每日壁纸、Pixiv、甚至 Telegram 的涩图频道（这里就不做推荐了）

* * *

参考教程：[优雅地下载我的B站投币视频](https://diygod.me/download-webhook/ "优雅地下载我的B站投币视频") [使用 rclone 将 Google Drive 文件同步至 OneDrive](https://cyhour.com/825/ "使用 rclone 将 Google Drive 文件同步至 OneDrive") [在Debian/Ubuntu上使用rclone挂载OneDrive网盘](https://www.moerats.com/archives/491/ "在Debian/Ubuntu上使用rclone挂载OneDrive网盘")
