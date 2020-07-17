---
title: Centos7上挂载Google-drive，给电脑加个大盘鸡！
tags:
  - 日志
  - 技术
excerpt: ''
date: 2018-04-28 22:36:44
---

众所周知，国内的某度云以及某某wei云等要么限速要么容量不够，关键它们对于用户隐私的窃取实在令人无法忍受，那么我们除了它们还能选择什么网盘呢？坚果云也许是个选择，但今天我非要讲google drive（因为我咸得太任性了），也就是所谓的谷歌网盘。 只要拥有一个google帐号，你就能拥有一个google drive，虽然容量并不大，但是有google account的人还怕这些吗？正常人都会想方设法去撸一个无限google drive，是吧！所以本文所有内容都基于你已经拥有了一个google-drive（无论有限或无限）且唯一遗憾的是需要特殊网络方能使用的假设！ 所需工具：VPS、 Google-drive、normal brain 步骤： 1.全部复制运行

    # 安装必要组件
    yum update -y
    yum install -y git
    yum install -y hg
    yum install sqlite-devel fuse fuse-devel libcurl-devel zlib-devel m4 -y
    # 安装 opam
    yum install ocaml ocamldoc ocaml-camlp4-devel -y
    wget https://raw.github.com/ocaml/opam/master/shell/opam_installer.sh -O - | sh -s /usr/local/bin/
    yum install 
    opam init 
    #安装 google-drive-ocamlfuse 这一步可能要10分钟左右

2.获取 Google Drive API mmmwhy12、12345替换为自己的。

    google-drive-ocamlfuse -headless -label me -id 1059522182613-89pc4hk9gv522889iteusipn4vpkppli.apps.googleusercontent.com -secret 1r7QyR35ATcOkz3KURCUq-9-

3.之后会出现一行链接，复制、粘贴到浏览器，打开 ![](http://www.feiyuyu.net/wp-content/uploads/2018/08/a39fe52fd8ee52596ef222e0bb899ce8.png)  

*   不能使用？ 你先需要进入到： [Google Drive获取一个API](https://console.developers.google.com/apis/api/drive.googleapis.com/)，类型就选择“其他”。
*   挂载 Google Drive   ![](http://www.feiyuyu.net/wp-content/uploads/2018/04/Q11180428223540.png)

*   卸载 ··· fusermount -u /home/google-drive ···
*   成功后便可通过在浏览器上访问VPS的IP地址，自由上传下载文件（即如果你的ip可在大陆直接访问，就跨越了GFW对google dive的高墙）