- VPN和VPS是什么

  1. VPN: 虚拟专用网络的功能是：在公用网络上建立专用网络，进行加密通讯。在企业网络中有广泛应用。VPN网关通过对数据包的加密和数据包目标地址的转换实现远程访问。VPN有多种分类方式，主要是按协议进行分类。VPN可通过服务器、硬件、软件等多种方式实现。
  2. VPS:（Virtual Private Server 虚拟专用服务器）技术，将一台服务器分割成多个虚拟专享服务器的优质服务

- 痛点: 之前由于一直需要科学上网工具，随购买VPN使用，速度倒是可以，但是由于在公司内部一旦连接上VPN，公司内网相关资源就无法访问，诸如git，mail，wiki等。需要再断开，VPN每次拨号时间虽然不长，但是也需要5s左右，由于经常需要频繁切换网络环境，较为繁琐，所以专用VPS内搭建SS给自己专线使用，而且ss支持动态代理，不需要代理的网络链接自动切换。而且速度对自己非常满意，并且月流量带宽远大于之前购买的VPN。鉴于搭建环境较为繁琐，特意记录一下，以便朋友们有需要可以参考。

- SS是什么?

> [Shadowsocks（中文名称：影梭）是使用Python、C++、C#等语言开发的、基于Apache许可证的开放源代码软件，用于保护网络流量、加密数据传输。Shadowsocks使用Socks5代理方式。 Shadowsocks分为服务器端和客户端。在使用之前，需要先将服务器端部署到服务器上面，然后通过客户端连接并创建本地代理。 在中国大陆，本工具也被广泛用于突破防火长城（GFW），以浏览被封锁、屏蔽或干扰的内容。在2015年8月22日，Shadowsocks原作者Clowwindy称受到了中国政府的压力，宣布停止维护此项目并移除其用户页面所载的源代码。[wiki](https://zh.wikipedia.org/wiki/Shadowsocks)

> ss相当于在本机和服务器之间建立一条隧道，可以定义哪些流量走隧道。ss不能保证身份的可匿。 _注: ss被屏蔽，以下称呼用$代替_

- 准备资源

  1. vultr帐号
  2. paypal或者vista信用卡
  3. mac(terminal) /windows putty

vultr 的有点，价格低，带宽大，优惠活动多，文档丰富，


![1.png](http://upload-images.jianshu.io/upload_images/239184-8f1f35910b73e132.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 最近vultr有优惠活动。新注册的用户将会或者20$的优惠活动，建议通过我的邀请链接<http://www.vultr.com/?ref=7021753-3B>去注册， 然后一定要支付金额才能使用。赠送的金额必须先充值才能使用vultr， 强烈建议充值10$,强烈建议充值10$,强烈建议充值10$,只有充值10$以上我才能收到邀请奖励，当作辛苦记录的稿费。

> 注: 不要试图试用之后去再申请帐号，vultr会检测支付帐号，如果重复申请帐号，使用了一个paypal去支付，会被vultr封号。

强烈建议使用paypal(类似中国的支付宝)去支付，优点是

1. 有的使用者并没有双币信用卡，paypal支持银联的卡片。
2. vultr目前无法解绑信用卡。

充值过程: 
![3.png](http://upload-images.jianshu.io/upload_images/239184-ee6f8023b85a57ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




- 购买一个服务器
![2.png](http://upload-images.jianshu.io/upload_images/239184-1d3c13be6f23f4a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![4.png](http://upload-images.jianshu.io/upload_images/239184-18d8adfdde3cd339.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里本人选择的是debin7 64位的操作系统，可能有些人偏爱，CentOS，Ubuntu，这些操作系统对我们搭建SS大同小异。价格选择5$即可，个人觉得满足5个人使用足够了，可以和小伙伴一起购买使用。

> 注： vultr的服务器只要添加就会开始收费，暂停是没用的，如果不需要使用了 一定要删除服务器。

之后等待install，进入running状态之后点击操作系统 

![5.png](http://upload-images.jianshu.io/upload_images/239184-a35035a49ff1e746.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![6.png](http://upload-images.jianshu.io/upload_images/239184-f45ceb68b871f44c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时候可以ping一下ip地址，如果可以ping的通就可以进行下一步了。 打开终端输入 ssh root@ip 询问回答YES，密码在server面板 点击复制，直接粘贴进终端，注意终端是不会显示密码的，并不是没有输入，。链接成功，如下图所示：

![7.png](http://upload-images.jianshu.io/upload_images/239184-25f021f275df502b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 安装$R就是ss的服务端。 简单的三个命令 分开输入。

```ash
1\. wget --no-check-certificate https://raw.githubusercontent.com/teddysun/$_install/master/$R.sh

2\. chmod +x $R.sh

3\. ./$R.sh 2>&1 | tee $sR.log
```



![8.png](http://upload-images.jianshu.io/upload_images/239184-e803c14123ecb028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![9.png](http://upload-images.jianshu.io/upload_images/239184-3209c0540aeed2d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![10.png](http://upload-images.jianshu.io/upload_images/239184-1eec73e0de479e1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





这里我密码输入的是123456， 端口号默认就好。 接下来自动完成后输出信息

![11.png](http://upload-images.jianshu.io/upload_images/239184-bcc76efc5a363b76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



现在我们的ss理论就好了，下面附上ssR相关的命令

```ash
/etc/init.d/shadowsocks status //检测状态
/etc/init.d/shadowsocks stop   // 关闭服务
/etc/init.d/shadowsocks start  //启动服务
```

如果有想修改密码 去编辑配置文件

```ash
vi /etc/shadowsocks.json
```

编辑完之后重启服务即可 有些朋友和小伙伴分享的时候习惯一人一个帐号，可以配置多帐号，下面是配置文件

```ash

{
    "server":"0.0.0.0",
    "server_ipv6": "[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "8989":"12345611",
        "8888":"123432311",
        "7777":"123ddsff45611",
        "8900":"478xcvx456456"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "protocol":"origin",
    "protocol_param": "",
    "obfs":"plain",
    "obfs_param": "",
    "redirect": "",
    "dns_ipv6": false,
    "fast_open": false,
    "workers": 1
}
```

但是本人亲测，有问题，不过一个帐号可以同时多人多设备在线，所以这里以后先挖坑，以后再填。

下面分享一个锐速的一键脚本，什么是锐速，总之是提升连接速度的好东西。安装即可

```ash

wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
```

装完之后自动会运行， 下面是检测锐速的命令

```ash
/serverspeeder/bin/serverSpeeder.sh status // 状态
/serverspeeder/bin/serverSpeeder.sh restart// 重启
```


![12.png](http://upload-images.jianshu.io/upload_images/239184-bc93a79e1792b062.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果想让锐速开机自启动 可以 vi /etc/rc.local 在最后输入 /serverspeeder/bin/serverSpeeder.sh start 保存即可

- 使用ss 先去下载ss对应的客户端软件 [ss下载](https://shadowsocks.com/client.html)

我这里下载mac端 ，iOS 由于被下架，只能去寻找替代品，在app store 输入 $ 搜索，里面有很多替代品，我下载的是netkit 1元， 也可以使用surge，之类替代的。

![13.png](http://upload-images.jianshu.io/upload_images/239184-d893629d9421016c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 打开即可科学上网。

注意 如果你是mac10.11之后，手动关闭了Rootless，貌似会导致无法上网，需要关闭。重启电脑即可。

- VPN补充

vps里面也是可以搭建VPN的，这里给一个简单搭建IPSpecVPN的教程

```ash
 1\.  wget --no-check-certificate https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/dev-debian/one-key-ikev2.sh

 2\.  chmod +x one-key-ikev2.sh

 3\.  bash one-key-ikev2.sh
```


![14.png](http://upload-images.jianshu.io/upload_images/239184-045daffb0c518e9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 这里输入1 其他默认即可



![15.png](http://upload-images.jianshu.io/upload_images/239184-3f041dab7204e847.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 开机自启动

```ash

vi /etc/rc.local
/usr/local/sbin/ipsec start
```

mac配置如下 

![16.png](http://upload-images.jianshu.io/upload_images/239184-8f3a13fe95024585.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 iPhone系统原生支持。

有时候系统vpn会挂掉，会导致ss也无法使用，解决办法就是重新安装vpn，。

- 免密码登录，ssh登录远程电脑需要输入密码， 这里可以配置免密登录，需要你的ssh配置，生成过程通github配置，没有配置可自行搜索

```ash
1   scp ~/.ssh/id_rsa.pub root@xx.xx.xx.xx:/root/id_rsa.pub // 第一句需要在本机执行。
2   cat id_rsa.pub >> .ssh/authorized_keys
3   chmod 600 .ssh/authorized_keys
```

执行即可。

--------------------------------------------------------------------------------

ss默认在终端下是无法翻墙的可以使用命令
  开启翻墙
 ` export ALL_PROXY=socks5://127.0.0.1:1080`

  关闭翻墙
 ` unset ALL_PROXY`

这些都是一次性的 
可以设置终端别名用来翻墙 

编辑 ~/.bash_profile
```sh

function proxy_off(){
    unset ALL_PROXY

    echo -e "已关闭代理"
}

function proxy_on() {
    export ALL_PROXY=socks5://127.0.0.1:1080
    echo -e "已开启代理"
}



```
执行`source  ~/.bash_profile`
下次使用`proxy_on `和`proxy_off`即可

----
有时候我们利用ss下载大文件还是比较慢，怎么办，我们可以远程在VPS上下载大文件，然后通过搭建一个web服务器，再通过专线下载回来即可。 写累了 先挖个坑，灾后续上。<br>
给我发个红包催更新吧


![weixin.png](http://upload-images.jianshu.io/upload_images/239184-cfbd591d9cd91486.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
