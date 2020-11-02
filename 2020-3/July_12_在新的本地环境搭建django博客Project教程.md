在新的本地环境搭建Django博客Project教程
---
假设已经根据[django博客教程]()成功搭建了自己的博客，并且已经将其发布到[github]()以及自己的私人服务器上。
现在需要在新的电脑或者环境上搭建一个管理该django博客的project，可以按照下面的步骤操作：

### 在本地还原博客环境

---
- 安装py3.5（ubuntu16.04会自带py3.5.2）。
- 安装pip3，使用[sudo apt-get install python3-pip]()。
- 安装virtualenv，使用[pip3 install virtualenv==16.6.1]()。
- 创建blogproject_env虚拟环境，使用[virtualenv home/vtoo/Document/blogproject_env]()，且因为是用的pip3安装的virtualenv，所以在这个环境里只能用py3.5.2。
- 激活blogproject_env虚拟环境，使用[source home/vtoo/Document/blogproject_env/bin/activate]()。
- 将在github上的blogproject的工程git到本地，使用[git clone url]()。
- 安装当前库依赖，使用[pip install -r requirements.txt](),会自动安装上博客project需要的库(如果报错,就按照requirements.txt目录,一个一个安装)。
- 迁移数据库模型，先使用[python manage.py makemigrations]()让django完成翻译，创建需要的迁移，之后使用[python manage.py migrate](),将各个应用的数据库模型进行迁移。如果之后对模型有修改，就依次使用[python manage.py makemigrations]()和[python manage.py migrate]()。
- 使用[python manage.py runsrver](),查看django是否可以正常运行，不报错的话就打开网址[http://127.0.0.1:8000/]()。
- 登录网址[http://127.0.0.1:8000/]()后报错[IndexError Exception Value:list index out of range](),这是因为当前数据库中没有**home，about，Todolist，contact，blog**的访问次数的初始化表格数据，但是远程服务器端是有的，这里需要在后台手动创建表格中的这五个数据，在代码中不能初始化，所以要先在代码中把这5个部分的view.py中的[refresh_visitnumber(request,'home')]()代码注释，分别对应报错的那几句代码，便可以正常运行project。
- 使用[python manage.py createsuperuser]()创建本地的后台管理账号，因为数据库的数据不会上传，所以这个和远程服务器端是不会冲突的，只用来管理本地的数据库。
- 进入后台，在后台**各页面de访问次数**中分别创建**home，about，Todolist，contact，blog**这5个数据，然后可以在代码中把先前注释了的代码复原，便可以完全正常运行博客project。
- **(可省略)**运行 [python manage.py collectstatic]() 命令收集静态文件到 static 目录下.
- 更简便的方法是,直接拷贝在其他本地电脑的blogproject,安装完py包之后就直接能用.

----

### 还原后首次修改博客发布到远端

----
- 在blogproject_env环境中查看本地blog的预览，方便修改blog的工程代码，此时的python版本是3.5的。
- 修改完blog的工程代码，使用[git status]() + [git add .]() + [git commit -m "xxx"]() + [git push -u]()将修改后的代码发布到github上。
- 因为fabric暂时不支持py3版本的，所以先退出blogproject_env环境，使用ubuntu自带的py2.7安装fabric，在终端使用命令[pip install fabric==1.14.0]()来安装(如果还没安装pip，就使用[sudo apt-get install python-pip]()先安装pip),注意，如果安装了高版本的fabric。可能会出错。
- 编写fabric脚本，即编写fabfile.py文件,放到博客工程文件夹，即[/home/vtoo/Document/blogproject/fabfile.py]()。其中的内容如下：

----

~~~python
# blogproject/fabfile.py

from fabric.api import env, run
from fabric.operations import sudo

GIT_REPO = "you git repository" ① 

env.user = 'you host username' ②
env.password = 'you host password'

# 填写你自己的主机对应的域名
env.hosts = ['demo.zmrenwu.com']

# 一般情况下为 22 端口，如果非 22 端口请查看你的主机服务提供商提供的信息
env.port = '22'


def deploy():
    source_folder = '/home/yangxg/sites/zmrenwu.com/django-blog-tutorial' ③

    run('cd %s && git pull' % source_folder) ④
    run("""
        cd {} &&
        ../env/bin/pip install -r requirements.txt &&
        ../env/bin/python3 manage.py collectstatic --noinput &&
        ../env/bin/python3 manage.py migrate
        """.format(source_folder)) ⑤ 
    sudo('restart gunicorn-demo.zmrenwu.com') ⑥
    sudo('service nginx reload')
~~~
**以上代码包含一些注解：**
~~~python
① 你的代码托管仓库地址。

② 配置一些服务器的地址信息和账户信息，各参数的含义分别为：
    env.user：用于登录服务器的用户名
    env.password：用户名对应的密码
	env.hosts：服务器的 IP 地址，也可以是解析到这个 IP 的域名
	env.port：SSH 远程服务器的端口号

③ 需要部署的项目根目录在服务器上的位置。

④ 通过 run 方法在服务器上执行命令，传入的参数为需要执行的命令，用字符串包裹。这里执行了两条命令，不同命令间用 && 符号连接：
	cd 命令进入到需要部署的项目根目录
	git pull 拉取远程仓库的最新代码

⑤ 对应上述部署过程中 3-5 的几条命令。因为启用了虚拟环境，所以运行的是虚拟环境 ../env/bin/ 下的 pip 和 python

⑥ 重启 Gunicorn 和 Nginx，由于这两条命令要在超级权限下运行，所以使用了 sudo 方法而不是 run 方法。
~~~


- fabfile.py文件写完之后，保存下来。用python2的环境运行fabfile.py文件，使用命令[fab deploy](),这时 Fabric 会自动检测到 fabfile.py 脚本中的 deploy 函数并运行。
- 如果在最后看到[Done. Disconnecting from zmrenwu.com... done](),说明脚本运行成功。而如果看到[Aborting. Disconnecting from zmrenwu.com... done.](),说明脚本运行中出错，检查一下命令行输入的错误信息，修复问题后重新运行脚本。
- 目前可能因为版本原因，fab的脚本一直有错误，目前采取的替代办法是，登录博客服务器，按照fabfile.py文件的指示，手动操作进行更新。

### 还原后非首次修改博客发布到远端

- 首先启动blogproject_env环境，用[source /home/vtoo/Documents/bolgproject_env/bin/activate]()来激活环境。
- 在blogproject_env环境中查看本地blog的预览，方便修改blog的工程代码，此时的python版本是3.5的。
- 修改完blog的工程代码，使用[git status]() + [git add .]() + [git commit -m "xxx"]() + [git push -u]()将修改后的代码发布到github上。
- 退出blogproject_env环境，使用python2的环境运行fabfile.py文件，使用命令[fab deploy](),这时 Fabric 会自动检测到 fabfile.py 脚本中的 deploy 函数并运行。
- 如果在最后看到[Done. Disconnecting from zmrenwu.com... done](),说明脚本运行成功。而如果看到[Aborting. Disconnecting from zmrenwu.com... done.](),说明脚本运行中出错，检查一下命令行输入的错误信息，修复问题后重新运行脚本。
- 目前可能因为版本原因，fab的脚本一直有错误，目前采取的替代办法是，登录博客服务器，按照fabfile.py文件的指示，手动操作进行更新。
- 目前主要开发环境已经迁移到台式机，台式机因为有anaconda，刚进入终端，会处在anaconda的base环境中，需要先退出到ubuntu自带的环境，再开启virtualenv的blogproject_env环境进行开发。

<center>
by [fuweifu](http://www.vtoo.pro)
<img src="https://pic.superbed.cn/item/5c83a7243a213b04178fed8c" width="10%" height="10%" />
</center>