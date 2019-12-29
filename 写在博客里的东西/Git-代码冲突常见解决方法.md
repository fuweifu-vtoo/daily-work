##Git:本地修改发布到服务器时代码冲突解决办法

情景模拟：电脑A和服务器B（或叫电脑B）同时git clone了某一个仓库，平时的工作状态是在电脑A修改代码之后发布到仓库，再由服务器B git pull下来。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但有一些配置文件（或其他文件）在服务器B上做了修改,然后后续电脑A开发又新添加一些配置项（文件）的时候,在由仓库到服务器B的过程中,会发生代码冲突:
		  
![代码冲突提示](http://pdcknxeeg.bkt.clouddn.com/%E4%BB%A3%E7%A0%81%E5%86%B2%E7%AA%81.png)

错误提示是：Your local changes to the following files would be overwritten by merge:  
&nbsp;&nbsp;blogproject/settings.py  
Please, commit your changes or stash them before you can merge.

下面有两种处理方式：  

1.想要保留服务器B的配置，又想将新的内容发布到服务器B的配置文件（仅仅在这个文件中并入新配置项）： 

```
git stash   
git pull  
git stash pop
```
这种方法也是 git 的 error 提示的办法，之后可以在服务器B终端用 **git diff -w +文件名** 来确认代码自动合并的情况.

2.如果希望用代码仓库中的文件完全覆盖服务器B的文件的版本，用下面的代码：

```
git reset --hard (+版本号)
git pull
```
其中git reset是针对版本回退,只要回退到某一个 blogproject/settings.py 文件没有被修改过的版本即可。

如果想只针对文件回退本地修改,使用：

```
git checkout HEAD file/to/restore
git pull
```


ps：这篇博客的情况要区别另一种情况：当服务器B在本地新增了文件（而不是修改了文件的某一行），并且这个文件是 **untrack** 的状态，此时发布到服务器B是不会出错的。（当这个文件是track的状态时，又另当别论了）

<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>