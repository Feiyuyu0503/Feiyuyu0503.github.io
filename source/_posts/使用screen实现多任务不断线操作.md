---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 技术
title: 使用screen实现多任务不断线操作
tags:
  - 技术
excerpt: ''
date: 2018-09-20 18:56:01
---

### 写在前面

我们用VPS执行某些命令时可能耗费的时间过长，如果出现掉线/断网的情况，我们还得重新执行命令。如果使用screen命令就不用担心这个问题。我们可以同时操作多个任务，也可以关机操作。

### 方法

1、安装：

    yum install screen #CentOS
    apt-get install screen #Debian或者Ubuntu
    

2、创建一个screen会话：

    screen -S xx #xx为创建会话的名称
    

3、隐藏并保留当前会话窗口： 按Ctrl+A，再按"D"键 如果怕中途掉线或者要离开的话，可以使用。 4、恢复会话窗口：

    screen -r xx #恢复名字为xx的会话
    

如果在恢复会话的时候忘记了或者没有设定会话名称我们就要执行：

    screen -ls
    

他会列出你所有的会话列表，然后使用：

    screen -r 会话名称
    

来恢复会话窗口。 5、关闭会话窗口：

    exit #或者下面一行，二选一
    screen -X -S [session # you want to kill] quit
    

screen的好处就是不会因为远程的操作因网络问题中断掉。

* * *

转自：[Rat's blog](https://www.moerats.com/archives/142/ "Rat's blog")
