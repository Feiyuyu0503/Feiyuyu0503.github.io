---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://blog.feiyuyu.net
categories: 软件
title: 太极（Taichi）——实现Xposed模块的新方法
tags:
  - 软件
excerpt: ''
date: 2019-02-20 22:50:08
---

太极已经有[官网](https://taichi.cool/README_CN.html "官网")了，写得肯定比我详细（逃 :huaji:

### 写在前面

太极分两种版本，一种太极普通版（不用解锁Bootloader，无需Root，不需要刷机就能使用 Xposed 模块的一个APP），另一种太极·Magisk版。

### 使用方法

**太极普通版** [酷安下载](https://www.coolapk.com/apk/me.weishu.exp "酷安下载") 首先添加应用到太极中 下载 Xposed 模块然后安装到你的手机上 在“模块管理”中启用需要的 Xposed 模块 强制停止对应的应用，Xposed 模块即可生效 _注意： 应用创建过程中必须首先卸载，然后才能正常安装；因此，请确保你已经对应用的数据做了备份。开发者不对任何数据丢失的情况负责。 应用创建后，可能与MIUI/华为等第三方ROM的系统分身功能冲突；如果需要使用太极，请删除全部系统分身应用。 某些Xposed 模块可能支持不好，后续会逐渐改进；_ **太极·Magisk版** [太极Magisk模块下载](https://www.lanzous.com/i31t25a "太极Magisk模块下载") 安装好 Magisk 刷入 太极 提供的 magisk 模块 安装好太极app 下与普通版同

### 两种版本的异同

太极·Magisk 是增强版的太极。 普通版的 太极 无法作用于系统APP，并且创建 APP 需要先卸载。太极·Magisk 借助一个 magisk 模块注入系统，从而可以作用所有的 APP；可以实现 Xposed 框架的完整功能。 具体来说，太极·Magisk 与普通版的太极有何区别？ 太极·Magisk 可以拦截系统，因此可以额外支持系统级别的 Xposed 模块；如边缘手势/核心破解/绿色守护/应用控制器等。 太极·Magisk 不需要修改 APP，因此可以做到 秒添加 APP；再也不用烦心地等待。 由于 太极·Magisk 不再需要修改 APP，因此 可以完美解决 微信/QQ 等应用无法被其他应用识别的问题。 太极·Magisk 有能力对系统做更多地控制，因此可以做到更加稳定。 与rovo89 的 Xposed 相比，太极·Magisk 又有什么特点？ 太极·Magisk 支持 Android 9.0。 太极·Magisk 不影响全局。可以只对特定的应用开启 Xposed 功能，无需使用 Xposed 的APP 运行起来就跟系统没有 Xposed 一样；完美契合某些金融/银行类APP。 太极·Magisk 针对APP的模块，无需重启即刻生效。 太极·Magisk 更不易被检测。太极的弱侵入特性不再修改 运行时，也不在全局环境中留下任何踪影；因此只要想要做到，可以轻松逃过各种代码类型的检测。

### 界面预览

![img](https://i.loli.net/2020/03/11/edKsr42METawNAF.png) ![img](https://i.loli.net/2020/03/11/TrByCai6k2uv8ZW.png)
