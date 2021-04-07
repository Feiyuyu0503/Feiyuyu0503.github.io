---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: 关于本站更换主题后bug的修复
tags:
  - 日志
  - 技术
excerpt: ''
date: 2018-06-27 22:06:04
---

1.用户ip显示cdn的ip ![](http://www.feiyuyu.net/wp-content/uploads/2018/06/829fd4189dd0f79dfca293cb71038f32.png) 解决方法： 在WordPress安装根目录下面wp-config.php文件，打开后，在其内添加下面代码：

    if(isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
      $list = explode(',',$_SERVER['HTTP_X_FORWARDED_FOR']);
      $_SERVER['REMOTE_ADDR'] = $list[0];
    }
    

2.评论时间显示错误（提前8小时） ![](http://www.feiyuyu.net/wp-content/uploads/2018/06/492d36fbeb6367673163f57b92a22755.png) 解决方法： 在wp-config.php中加入：

     date_default_timezone_set('Asia/Shanghai');
    

并且在后台设置中把时间改为UTC世界标准
