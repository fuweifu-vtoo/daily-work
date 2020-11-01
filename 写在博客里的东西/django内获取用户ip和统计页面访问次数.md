##django内获取用户ip和统计页面访问次数
自己的博客总算也是搭建成形了，但还有很多地方需要完善，需要自己不断的给它加上各种功能。  
例如，我希望能够知道有多少人访问了我的网站,这个功能可以使用第三方平台来做，例如“百度统计”，“诸葛io（基于cnzz）”和“cnzz”等，但是现在我想先自己用代码实现一下，想法是把这些数据存到我的数据库，在后台进行管理。  
具体实现如下：  

不想在django中创建太多应用，就把用户ip的访问信息 **Userip** 和页面的访问次数 **VisitNumber** 放到其他应用中，首先在一个models.py中加入下面的代码：  

```
#访问网站的ip地址和次数
class Userip(models.Model):
    ip=models.CharField(verbose_name='IP地址',max_length=30)    #ip地址
    count=models.IntegerField(verbose_name='访问次数',default=0) #该ip访问次数
    class Meta:
        verbose_name = '访问de用户信息'
        verbose_name_plural = verbose_name
    def __str__(self):
        return self.ip

#各页面访问次数
class VisitNumber(models.Model):
    page_name = models.CharField(verbose_name='页面名称',max_length=30,default='home')
    count = models.IntegerField(verbose_name='页面访问次数',default=0) #网站访问总次数
    class Meta:
        verbose_name = '各页面de访问次数'
        verbose_name_plural = verbose_name
    def __str__(self):
        return str(self.count)
```
此后可以运行下面两句话更新django的数据库表：

```
python manage.py makemigrations
python manage.py migrate
```
完成之后，要写对 **Userip** 和 **VisitNumber** 两个类操作的函数，这两个函数可以写到 views.py 中，也可以写到一个新建的 .py 文件，之后再在 views.py 文件中 import 即可，两个操作两个类的函数分别如下：

```
from .models import VisitNumber,Userip

#自定义的函数，不是视图
def refresh_visitnumber(request,page_name):       #修改网站访问量和访问ip等信息
    # 每一次访问，网站总访问次数加一
    count_nums = VisitNumber.objects.filter(page_name=page_name)
    count_nums_pagename = count_nums[0]
    count_nums_pagename.count += 1
    count_nums_pagename.save()

def get_Userip(request):
    # 记录访问ip和每个ip的次数
    if 'HTTP_X_FORWARDED_FOR' in request.META:  # 获取ip
        client_ip = request.META['HTTP_X_FORWARDED_FOR']
        client_ip = client_ip.split(",")[0]  # 所以这里是真实的ip
    else:
        client_ip = request.META['REMOTE_ADDR']  # 这里获得代理ip

    ip_exist = Userip.objects.filter(ip=str(client_ip))
    if ip_exist:  # 判断之前是否存在该ip
        uobj = ip_exist[0]
        uobj.count += 1
    else:
        uobj = Userip()
        uobj.ip = client_ip
        uobj.count = 1
    uobj.save()
```
（真的很不会起名字，我对起名字的套路就是不断的用下划线往后加...：）  
代码中都有注释，其中refresh_visitnumber()可以对页面的访问次数做统计，get_Userip()可以获得用户ip，并且记录ip的访问次数。
之后，在需要的地方调用refresh_visitnumber()和get_Userip()函数就可以了，如下： 

1.在视图函数中调用refresh_visitnumber()：
![](http://pdcknxeeg.bkt.clouddn.com/1.png)
2.在类视图函数中调用refresh_visitnumber()和get_Userip()函数，需要注意的是，两个函数都需要 request 参数，但是类视图函数中是不传递 request 参数，但可以传递 self.request，如下图：
![](http://pdcknxeeg.bkt.clouddn.com/2.png)

到这一步，我以为就已经结束了，可是当将代码发布到服务器中测验时，发现页面的访问次数正常，但时会获取到空的用户ip，如下图：
![](http://pdcknxeeg.bkt.clouddn.com/3.png)
其中 112.97.57.209 是我手机当时的ip地址，而这个空的ip访问了11次，都是我用电脑访问的11次。  
我想知道是不是只有我的电脑会这样，于是我到django中文网群（10218442）中请群友帮我测试我的网站，发现都识别为空ip，空ip的访问次数一直在增加，如下图：
![](http://pdcknxeeg.bkt.clouddn.com/4.png)

最后解决办法是通过群里的一位前辈指点：

<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/5.png" width="60%" height="60%" />
</center>

原来是nginx的反向代理的原因，通过下面代码轻松设置一下nginx的配置文件即可（在原有的nginx配置文件中加入下面三行代码）：

```
location / {

//原有的两行代码，第一行可能会不同，没有关系
　　　proxy_pass http://127.0.0.1:10678;

　　　proxy_set_header Host $host;

//增加的三行代码
     proxy_set_header X-Real-IP $remote_addr;

     proxy_set_header REMOTE-HOST $remote_addr;

     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
```
通过增加这三行代码，就可以获得客户端的真实ip地址！！效果图如下：
![](http://pdcknxeeg.bkt.clouddn.com/6.png)
:)所以啊，以后不要轻易访问我的网站，不然就被我知道ip了（其实ip一直都在变，并没有什么用处...），还有啊，我的网站才不止这么多浏览量，我只是随便截的图，真的！(*｀へ´*)

<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>



