##Linux下创建／删除新用户并赋予root权限
为新的操作系统多创建新用户，并给超级权限，步骤如下： 

```
#确保在一个具有root权限的账户下运行命令
useradd -m -s /bin/bash fuweifu

#把新创见的用户加入超级权限组
usermod -a -G sudo fuweifu

#设置新用户的密码
passwd fuweifu

#到这一步，已经创建成功，并且赋予了超级权限，此时可以切换到fuweifu
su - fuweifu
```

使用useradd命令所建立的账号，实际上是保存在/etc/passwd文本文件中。   
下面展示一些 useradd 的参数使用： 
 
```
useradd
1.作用
useradd命令用来建立用户帐号和创建用户的起始目录，使用权限是超级用户。
2.格式
useradd [－d home] [－s shell] [－c comment] [－m [－k template]] [－f inactive] [－e expire ] [－p passwd] [－r] name
3.主要参数
－c：加上备注文字，备注文字保存在passwd的备注栏中。　
－d：指定用户登入时的启始目录。
－D：变更预设值。
－e：指定账号的有效期限，缺省表示永久有效。
－f：指定在密码过期后多少天即关闭该账号。
－g：指定用户所属的群组。
－G：指定用户所属的附加群组。
－m：自动建立用户的登入目录。
－M：不要自动建立用户的登入目录。
－n：取消建立以用户名称为名的群组。
－r：建立系统账号。
－s：指定用户登入后所使用的shell。
－u：指定用户ID号。
4.说明
useradd可用来建立用户账号，它和adduser命令是相同的。账号建好之后，再用passwd设定账号的密码。使用useradd命令所建立的账号，实际上是保存在/etc/passwd文本文件中。
5.应用实例
建立一个新用户账户，并设置ID：
＃useradd caojh －u 544
需要说明的是，设定ID值时尽量要大于500，以免冲突。因为Linux安装后会建立一些特殊用户，一般0到499之间的值留给bin、mail这样的系统账号。
```  
下面给出一个更直观的例子：

```
在终端里执行以下命令：
 useradd -d /home/"username" -g "gid" -u "uid" -m -s /bin/bash "username"
 passwd "username"
#“username"自己指定， ”gid"必须是现有的组id，“uid"必须目前未被使用
#/etc/group文件里有所有组信息。
# 以下命令可以创建新组：
 groupadd -g "gid" "group name"      
```
当需要删除一个用户时，使用 userdel 删除：  

```
格式：
userdel -r -f fuweifu
参数：
-f :强制删除用户，即使用户当前已登录；
-r :删除用户的同时，删除与用户相关的所有文件(家目录的所有文件)。

```
不要轻易用-r选项，用户目录下可能有重要的文件，在删除前记得备份。  
刚刚提到，useradd命令所建立的账号，实际上是保存在/etc/passwd文本文件中，所以删除用户时，也可以直接在/etc/passwd中删除想要删除的用户记录。  
删除之后，用户就不能登录了。
<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>