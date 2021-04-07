---
author: feiyuyu
photos: https://api.btstu.cn/sjbz/api.php?lx=dongman&format=images
avatar: 'https://cdn.jsdelivr.net/gh/feiyuyu0503/cdn@4.0.0/img/avatar/avater.jpg'
authorLink: https://www.feiyuyu.net
categories: 技术
title: GcmForMojo--QQ&Wechat的消息推送
tags:
  - 技术
excerpt: ''
date: 2018-12-06 20:38:54
---

### 凉了 永远怀念

[webQQ走到终点：2019年1月1日起停止服务](https://www.cnbeta.com/articles/tech/797689.htm "webQQ走到终点：2019年1月1日起停止服务")

![gcm.png](https://i.loli.net/2018/12/14/5c13b1ca5e8ba.png "gcm.png")

### 写在前面

GcmForMojo ，功能是可以将QQ和微信的消息通过FCM/GCM/MiPush/HwPush/MzPush推送到你的手机，所以效果就是省电，省流量啊，当然你需要有一个VPS。 不愿意搞VPS的机友可以尝试在旧安卓手机或者平板部署服务端,本文不再赘述，[查看](https://github.com/lgcde/mojo-qqwx/blob/master/README.md "查看")

### 使用教程

[安卓客户端下载](https://www.coolapk.com/apk/com.swjtu.gcmformojo "安卓客户端下载") 服务端架设：本教程环境为 CentOS 6.8 ，其他 Linux 发行版请自行修改对应命令。本文以Mojo::WebQQ为例。 首先安装依存关系

    yum -y groupinstall "Development Tools"
    yum install vim git openssl-devel perl cpan make gcc g++
    

由于我们需要 Mojo::WebQQ 这个 Perl 语言项目作为主机端接收QQ消息的平台，同时 CentOS 6.8 自带 yum 源没有 Cpanm 包管理，所以接下来需要安装 Cpanm

    cpan -i App::cpanminus
    

Cpanm 安装完毕后，我们就可以安装 Mojo::WebQQ/Mojo::Weixin 了。

    cpanm Mojo::Webqq
    

Mojo::WebQQ安装完毕后，就可以开始进行设定了 首先我们需要新建一个 Perl 脚本文件来执行

    touch GCM.pl
    

我们使用 vim 对文件进行操作

    vi GCM.pl
    

这里给出一个 Perl 脚本文件的模板，具体内容请根据实际情况替换更改

    use Mojo::Webqq;
    #微信使用 use Mojo::Weixin
    my $client = Mojo::Webqq->new(log_encoding=>"utf-8");
    $client->load("ShowMsg");
    #请根据自己所需的推送服务进行选择并删除或注释不需要的部分，填写格式请仿照 GCM 的方式填写
    #以下为 GCM 推送
    $client->load("GCM",data=>{
        api_url => 'https://gcm-http.googleapis.com/gcm/send',
        api_key=>'AIzaSyB18io0hduB_3uHxKD3XaebPCecug27ht8',
        registration_ids=>["输入你自己从 GCMForMojo APP中获取到的令牌"],
        allow_group=>["接受群消息的号码，如需要推送全部群消息可删除这一行，每个群号码之间使用 "", 分隔"],
        ban_group=>[],
        allow_discuss=>[],
        ban_discuss=>[],
        #此处为讨论组，填写格式同上
    });
    #以下为 MiPush 推送
    $client->load("MiPush",data=>{
        registration_ids=>[""],
        allow_group=>[""],
        ban_group=>[],
        allow_discuss=>[],
        ban_discuss=>[],
    });
    #以下为 HwPush 推送
    $client->load("HwPush",data=>{
        registration_ids=>[""],
        allow_group=>[""],
        ban_group=>[],
        allow_discuss=>[],
        ban_discuss=>[],
    });
    #以下为 FmPush 推送
    $client->load("FmPush",data=>{
        registration_ids=>[""],
        allow_group=>[""],
        ban_group=>[],
        allow_discuss=>[],
        ban_discuss=>[],
        });
    $client->load("Openqq",data=>{
        #如果是微信改为 Openwx
        listen => [{host=>"0.0.0.0",port=>5000}, ] ,
        #如果是推送微信的话需要保证端口不重复，并请保证所设定的端口已经在防火墙内放行，同时需要在 APP 内设定好推送服务器的地址和端口
    });
    #不需要 APP 内回复功能请删除以上三行（不包括被 # 号注释掉的几行）
    $client->run();
    

保存退出后执行

    perl GCM.pl
    

这时候你的 GCMForMojo APP 应该会弹出一条检测到二维码事件的通知，点击它，使用手机端 QQ 扫描这个二维码，你的 GCMForMojo 就跑起来了

### 注意事项

1.Perl 进程并不会后台运行！！！同时如果你的 SSH 连接中断的话当前终端下运行的全部会话均会被杀死，若想保持后台运行且断掉 SSH 连接后依旧可正常工作，请使用 screen 命令 `screen -S docker perl GCM.pl` 然后请按 Ctrl+A ，再按 D 键使此 screen 进入后台驻守，然后就可以中断 SSH 连接了，如果需要恢复此 screen 的话，请执行 `screen -r docker` 更多screen使用请查看[使用screen实现多任务不断线操作](http://www.feiyuyu.net/archives/1243 "使用screen实现多任务不断线操作") 2.由于 OpenQQ 组件使用 HTTP 请求而不是更安全的 HTTPS 请求，尤其像现在免费WiFi盛行 通过HTTP传送极易被窃取消息（数据未被加密），OpenQQ端口默认没有鉴权，任何人都可以调用OpenQQ接口以该QQ的身份向外发送消息（未认证的访问）这样会很容易被他人监听，并有可能以你的身份发送消息，所以为了安全起见，强烈建议以加盐或开启 HTTPS 加密的方式增强安全性 ① 加盐 在 GCM.pl 文件内加入以下内容

    use Mojo::Webqq;
    use Digest::MD5  qw(md5 md5_hex md5_base64);
    #请确保上一行加入在文件头行，否则会报错
    #以下略
    $client->load("Openqq",data=>{
        listen => [{host=>"0.0.0.0",port=>5000}, ] ,
        auth => sub {
            my($param,$controller) = @_;
            my $secret = 'salt';
            #请将该行salt改为你自定义的盐值，并在 Android 端内设定好你所自定义的盐值
            my $text='';
            foreach $key (sort keys %$param){
                if($key ne 'sign'){
                    $value =$param->{$key};
                    $text.=$value;
                }
            }
            if($param->{sign} eq md5_hex($text.$secret) ){
                return 1;
            }
            else{
                return 0;
            }
        }
    });
    以下略
    

② HTTPS 加密 在 GCM.pl 文件内加入以下内容

    #以上略
    $client->load("Openqq",data=>{
        listen => [{
        host =>"0.0.0.0",
        port =>443,
        #请求监听端口，默认443，也可以自定义
        tls =>1,
        #开启https请求支持
        tls_ca  =>"/etc/tls/ca.crt",
        #可选，ca证书路径
        tls_cert=>"/etc/tls/server.crt",
        #服务器证书路径
        tls_key =>"/etc/tls/server.key"},],
        #证书对应的key文件
    });
    

服务端设定好后把 Android 端的服务器上将 [http://xxxxxx.xx:8080](http://xxxxxx.xx:8080) 改为 [https://xxxxxx.xx:8080](https://xxxxxx.xx:8080) 即可 注意，如果需要 HTTPS 加密，你需要申请一个域名并绑定在你的 推送服务器 上，否则你是无法签发可被信任的证书的，除非你选择了 自签发证书 ,但这样做会更麻烦。 3.大概是出于QQ官方的限制，web端qq并不稳定，可能隔一段时间就会掉线，下面给出对于Mipush推送的玄学解决方案。 适用于接收不到推送/推送延迟/推送失灵问题 原因：vps网络连接米推接口出现问题，导致推送从服务器端无法正常提交。情况体现为服务器能接收信息，mojo端配置正常能回复信息却不能接收信息，或者间歇性失灵都是接口解析地址变动导致。 解决方案：修改服务器/etc/hosts 添加hosts文件新行为 \[ip\] api.xmpush.xiaomi.com

\[ip\]的替换按如下操作: 阿里云/腾讯云 203.100.92.33或183.84.5.32 欧洲vps:183.84.5.32其余vps:120.92.96.12 手机/路由器作服务器:203.100.92.33或118.26.252.19 如修改后仍不起作用，统一修改为183.84.5.32

更多细节请查看：[Github](https://gist.github.com/kotomei/5367a003cd16d05e075c21a7f360b09a "Github")
