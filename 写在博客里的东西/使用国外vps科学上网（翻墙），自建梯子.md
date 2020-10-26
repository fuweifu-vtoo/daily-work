## 使用国外vps科学上网（翻墙），自建梯子

最近想体验一下资本主义国家的“空气”，周围朋友用的多的是收费的VPN。不过最近在搭建博客时，听说可以用vps自建梯子翻墙（很牛批的样子），而且还有不用花钱租vps的办法，于是体验了一下。

不得不说，这个真的是又快又好，还没有时间和流量限制，且一台vps理论上不限制登录的设备和账户，也就是只要我搭建成功，周围人就可以用我搭建的梯子免费翻墙。

首先介绍什么是vps，vps是一台拥有公网IP的服务器，购买国外vps的厂商有很多，例如Vultr、搬瓦工、DigitalOcean等～

首先我们需要一台国外的vps，可以在DigitalOcean上“购买”，因为DigitalOcean最便宜的只要5刀每月，**而且通过[点此](http://www.digitalocean.com/?refcode=78a4769048ee)进官网购买注册会得到10美刀**，也就相当于可以免费使用两个月。

进入后是这个界面：
![欢迎界面](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8B%E5%8D%888.22.05.png)
创建账户登录后，会有英文提示，提示你如果绑定visa的信用卡，账户会免费充值10刀，有效期12个月（2018.8.11）。
![10美元](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.15.49.png)

按照提示绑定一张自己的visa信用卡，查看账户余额，发现真的有10$被充值进去了。
![10meiyuan](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.34.28.png)
下面正式开始搭建梯子。首先创建一台5刀配置的服务器。
![ubuntu配置](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.25.49.png)
我这里选择ubuntu系统，用的人最多，出故障了更容易找到解决方案。
![ubuntu](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.25.41.png)
所有东西选好之后之后，就可以看见自己刚刚创建的服务器，并且这个时候还没有扣费，账户里还是10刀。
![ubuntu-do](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.31.18.png)
但是我发现我只知道我服务器的公网ip地址，并不知道密码...,原来和国内购买的一些服务器不同，digitalocean需要手动重置密码，之后密码会发到注册的邮箱账户中：
![password](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%889.38.37.png)
知道密码后，便可以登录服务器配置搭建梯子的工具，登录服务器可以用digitalocean最右端自带的控制台，也可以在本地ssh远程登录，推荐后者，因为自带的普遍比较卡。

如果使用的是windows系统，可以使用git bash软件远程ssh连接，点击[此处](https://git-scm.com/downloads)下载。
如果使用的是linux或者mac系统，可以忽略上面的git bash，直接在终端ssh登录vps，登录指令如下：

```
ssh -q -l root -p 22 123.123.123.123
```
其中123.123.123.123替换成自己服务器的ip，root可以不用换，因为digitalocean的默认登录名是root，当然这个是可以改的，只要最后成功登录上就可以。

因为是新服务器，最好先更新一下系统，避免因为版本太旧而给后面安装软件带来麻烦。**依次**运行下面的两条命令：

```
sudo apt-get update 
sudo apt-get upgrade 
```
更新完系统之后，要开始安装这次的主角：**shadowsocks**。翻墙主要是围绕它运行的，意思就是在一个国外vps上运行**shadowsocks**，然后设备就可以通过vps上网。依次运行下面的命令：

```
apt install pip
pip install --upgrade pip 
pip install shadowsocks
```
运行到第三条指令时，会出现错误：Importerror:cannnot import name main（如下图），这是pip升级后存在的问题，需要修改一些pip的文件，非常easy，参考[此处](https://www.cnblogs.com/white-the-Alan/p/8900554.html)解决bug。

![pip error](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%8810.00.37.png)


解决pip的bug之后，重新运行“ pip install shadowsocks ”，可以正常安装好shadowsocks。下面要对shadowsocks进行一些配置，使得翻墙能够成功。

编辑一下/etc/shadowsocks.json文件，命令如下：

```
vi /etc/shadowsocks.json 
```
执行上述命令后，就进入文件编辑模式，这是自己创建的一个新的空白文件，点击键盘上的 i 进入插入模式，便可以输入代码，按照下面模板输入以下代码：

```
{
    "server":"0.0.0.0",
    "server_port":8388,
    "password":"yourpassword",
    "timeout":600,
    "method":"aes-256-cfb"
    "fast_open":false
    "workers":1
}
```
其中需要修改的是“password”这一项，改成你的密码，这是用于登录shadowsocks的密码，和之前创建vps的密码还有digitalocean的密码不是同一个，这是三个不同的密码。

ps一点：/etc/shadowsocks.json中的代码最好是手动输入，之前我选择粘贴复制的时候，到下一步用python解析的时候一直会报错，因为有看不见的隐藏符号，尤其是去复制windows下的代码到其他系统，经常会有这个编码问题。


添加完之后，按键盘上“esc”，再按键盘上的“：”，输入“wq”，退出vi编辑模式。

最后，使用下面的指令运行shadowsocks：

```
ssserver -c /etc/shadowsocks.json -d start
```
运行完之后，会出现下面的界面:

![success](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-12%20%E4%B8%8A%E5%8D%8811.05.55.png)

下一步是进行bbr加速，同样可以翻墙，但是翻墙的速度很影响用户体验，为了看YouTubu的1080的视频不卡顿，必须加速，于是就有了bbr加速，直接在终端依次运行下面的指令，有时弹出交互式对话框，直接按照默认项选择就好：

```
wget –no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh  
chmod +x bbr.sh
./bbr.sh
```

至此，shadowsocks服务端就配置运行成功了！ 

到这一步就已经完成了大半，下一步是在你需要翻墙的设备上安装shadowsocks的客户端用于连接vps，从而翻墙，这里给出不同客户端的下载地址：

[Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)

[Linux](https://github.com/shadowsocks/shadowsocks-qt5/releases)

[mac](http://pdcknxeeg.bkt.clouddn.com/shadowsocksx-2.6.5.dmg)

[android](https://github.com/shadowsocks/shadowsocks-android/releases)

苹果手机可以在应用商店下载shadowrocket或者wingy，应用商店如果没有，据说可以用pp助手下载（穷人家没有iPhone也不确定...）

这里我用手机android演示如何连接，下载shadowsocks之后，在下面的页面配置信息：
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/WechatIMG45.jpeg" width="50%" height="50%" />
</center>
只需要配置前四项的信息，即vps的公网ip、端口号、shadowsocks的密码（在/etc/shadowsocks.json中配置的密码）、加密方式（采用默认AES-256-CFB）。

输入完成之后，点击右下角一个飞机✈️的小图标，图标变亮，并且还有显示下载和上传速度，则翻墙成功，下面展示一张效果图：
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/WechatIMG91.jpeg" width="50%" height="50%" />
</center>
然后，你就翻墙成功了，在此期间，本地连接vps的终端可以关掉，但是vps本身不能关机，因为翻墙是借助vps的国外ip，如果关机了，就自然不能翻墙。

很简单有没有，而且前两个月还是免费的，在此基础上，有兴趣的还可以折腾一下vps，因为vps不仅仅是可以用来翻墙的，vps本身是一台虚拟服务器，还可以利用vps学习linux操作系统，也可以在上面搭建自己的个人网站（如果不是我心急，很早就买了腾讯云一年的服务器，我就会在vps上搭建个人网站）。

另外，除了在digitalocean上有绑卡优惠之外，在Vultr上也有优惠，使用下面的注册地址，可以获得10美元的代金劵（这是我的邀请码，我也会有奖励的～）：

[Vultr]()注册地址：[https://www.vultr.com/?ref=7505345](https://www.vultr.com/?ref=7505345)

并且Vultr最近出了一个 “绑定visa信用卡送25美元” 的活动（居然比digitalocean还要优惠hhh~），点击活动地址：[https://www.vultr.com/promo25b](https://www.vultr.com/promo25b)  
本次活动仅限新注册用户，老用户不行，充值方式是 PayPal 或 信用卡，支付宝支付不能参与本次活动，而且PayPal和信用卡在Vultr上从未支付过。

活动没有具体截至时间，随时都可能截至(2018.8.12还没有截止)，送的25美元有效期为1年，所以我立刻去注册并且绑卡，现在我的账户又多了25美金，这钱来的太快～
![vultr账户余额](http://pdcknxeeg.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-08-13%20%E4%B8%8A%E5%8D%889.38.44.png)

所以，有一张visa的信用卡真的很有用，谢谢当初和我一起去办信用卡的某同学(ಡωಡ)。
<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>





