## 在服务器上搭建Django博客Project教程
---
假设已经根据[django博客教程]()成功搭建了自己的博客，并且已经将其发布到[github]()，但是还未发布到自己的私人服务器上。
现在需要在服务器上搭建django博客，方便他人访问，可以按照下面的步骤操作：

### 在本地还原博客环境
参考之前写过的教程[在新环境搭建Django博客Project教程]()。

### 提前注释会报错的代码
之前在本地访问博客时报错[IndexError Exception Value:list index out of range](),这是因为当前数据库中没有**home，about，Todolist，contact，blog**的访问次数的初始化表格数据，这里需要在后台手动创建表格中的这五个数据，在代码中不能初始化，所以要先在代码中把这5个部分的view.py中的[refresh_visitnumber(request,'home')]()代码注释，分别对应报错的那几句代码，便可以正常运行。

### 在服务器搭建博客环境
参考教程[使用 Nginx 和 Gunicorn 部署 Django 博客](https://www.zmrenwu.com/courses/django-blog-tutorial/materials/15/)

### 搭建过程中要注意的点（踩过的坑)

1. 可以使用服务器ipv4地址直接搭建成功，如果是域名还需要实名认证和备案，所以搭建的前期就使用ipv4地址搭建就好了。
2. 安全组设置，否则输入ip访问不了；
3. 域名备案；
4. 一直显示welcome to nginx，在ngnix的sites-avaiable处有两个地方要改，www.vtoo. pro.socket不要忘记了，在gunicorn中也要相应变化，且必须是www.vtoo.pro.socket，不能是别的，否则一直是welcome to nginx
5. 502 badway错误，没有用gunicorn启动django服务；
6. 必须先 makemigration 再 migrate，这里博客上面漏了第一步，会出现no table的错误
7. 有时必须重启服务器，或重启nginx，就能成功
8. allowed_hosts中，添加服务器ipV4地址，否则无法访问
9. 使用gunicorn启动django服务时的语句必须是：gunicorn --bind unix:/tmp/www.vtoo.pro.socket blogproject.wsgi:application
10. ubuntu16.04已经不再支持upstart了，也就是不能通过upstart设置gunicorn自启动，新版本支持的是supervisor(未配置成功).
11. ==目前只能使用gunicorn管理，但是不能设置gunicorn自启动，也就是如果gunicorn的进程运行太久，万一被杀死，网站就会崩溃。==
12. fabric自动部署也因为版本问题未配置成功(大概率是笔记本电脑上pip和包的依赖的原因，在台式上可以再试一次)，目前若要更新博客代码，需要
	- 本地修改代码并测试
	- 上传至github
	- 按照fabfile.py中的内容，到服务器中手动更新
	- 重启nginx：service nginx reload
13. 有针对 ubuntu16.04 对 upstart 和 python2版本fabric 无法正常使用的问题进行改进的两个博客：[我的Django-blog学习(三)：blog服务自动启动服务脚本 Gunicorn](https://blog.csdn.net/qq_41854273/article/details/83343053),[我的Django-blog学习(四)：使用 Fabric3 自动化部署](https://blog.csdn.net/qq_41854273/article/details/83344255),但是都未配置成功。

### 使用七牛云存储作为图床

1. 七牛云作为图床需要对自定义的二级域名ICP备案才行，参考博客[网站域名、备案、七牛云图床重新搭建与博客整理](https://www.jianshu.com/p/ebed904d852d)
2. ICP备案和网安备案(公安局备案)都要做。

### 代码做过的改动

1. icon的原css文件失效，使用新的`<link rel="stylesheet" type="text/css" media="screen" href="https://cdn.staticfile.org/ionicons/2.0.1/css/ionicons.min.css">`代码替换，在base.html中替换。
2. 修改TodoList的顺序，放到了About和Contact之前。
3. Blog页面的颜色和背景在base.html中的第51行，关键在于id="main"，后面第68行，`<body/>`中有用到。在base_without_aside.html中同理。
4. Blog页面模块的透明度和圆角用代码：`style="filter:alpha(opacity:30); opacity:0.7;border-radius: 10px 10px 10px 10px;"`实现，同样在base.html中，有多处使用到了。
5. TodoList页面的背景颜色修改在css文件中，查找时应看准对应的id；blog的背景颜色设置也在css当中，但是通过自定义style，在base.html中覆盖了css中设置的颜色。
6. 删除了音乐播放器的控件，在base.html中。
7. 把blog页面的menu控件删除，在手机端无法用menu，以及用vtoo代替'返回home'的作用。
8. 在blog/css/custom.css修改了部分代码，主要是blog界面的显示，要修改blog界面的样式，就在custom.css中找对应的class.

------
### todo

1. 邮件系统现在还不能发送邮件。   |   完成

2. todolist中定位信息有误。		|　放弃

3. about里面的个人介绍介绍一下自己。	| 完成

准备在寒假重新使用django+服务器搭建自己的网站，到时候还会对比使用hexo+github.io搭建博客。

到时候会有很多版本问题，需要一个个解决。

除此之外，我还会更加系统的学习一下django，依旧是参考追梦人物的博客[HelloDjango - Django博客教程(第二版)](https://www.zmrenwu.com/courses/hellodjango-blog-tutorial/),这是一套Django全栈开发系列教程，现在还在连载中。