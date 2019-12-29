# July_3_ubuntu的find_cat_gdebi_dkpg命令
------
##some about ubuntu
- ++find / -name "\*libgtk\*"++ 可以在ubuntu中查找文件，++-name++表示的是用名字查找，发现比较适合的find_cat_gdebi_dkpg命令我平时的需求。

- ++sudo apt-get update++和++sudo apt-get upgrade++的更新操作实际上是根据software update中的文件直接更新的，它会逐一去链接ubuntu源和第三方软件的ppa源（如果有某一个第三方ppa源链接不上，会一直卡在打开这个软件），可以删除和更改源。例如，一般新装的系统是换成aliyun源。

- tweak和gnome都可以对ubuntu进行美化，tweak用的人多，我现在使用的是里面的一款扁平化主题flatabulous。并且终端的透明度是可以调整的。

- ++cat /etc/issue++可以把ubuntu的版本号输出来，我的是16.04.6

- gdebi可以安装.deb的安装包，这样就不用使用dpkgn来安装了，原本是用的新立得软件包管理器（synaptic software manager）安装。安装的代码示例：++sudo gdebi XXX.deb++
- ++dpkg -l | grep libgtk++可以查看装的有关libgtk的所有包的版本，以及他们适合的属性，这个和查看显卡驱动时的命令类似++lsmod | grep nouveau++，后来搜了一下，grep是一个很强的命令，可以用来搜索字符串。**grep**

- 当ubuntu不能访问wins的盘符的时候，使用sudo ntfsfix /dev/sda1来进行修复

- ubuntu中，prtsc可以用来截屏，shift+prtsc可以用来截特定一块的区域。

- ++which python++可以查看当前使用的Python的版本和路径。

------