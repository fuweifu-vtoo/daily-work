#July 6（装机review）
--------
##装机review

总结一下自己装机的心路历程，总算是使用了uefi+gpt装机双系统成功。

###遇到的问题
1. legency+mbr装机时，ubuntu的引导特别麻烦，会先进win10的引导，进入easyBCD选择ubuntu，之后电脑重启加载ubuntu的引导，之后是选择ubuntu，这样才能进入ubuntu。

2. 而且我这种方式装上的ubuntu非常卡，分辨率也不行，并且装不上nvidia的驱动。

3. 之前使用的是legency+mbr装机，两者的差别很大，而且uefi+gpt是新的装机方式，legency+mbr装机是老的，所以我就很想改过来。
###legency+mbr装机
总结上面的装机方式，legency+mbr装机比较过时，主要是因为每次启动要运行主板自检，还有使用的是grub，而uefi用的grub2，开机时间在25秒左右，安装双系统时，要使用easyBCD引导一下ubuntu的引导项，ubuntu的引导加载程序装在/或者单独分一个/boot分区。
###uefi+gpt装机
而uefi+gpt装机方式启动更快，大概10s左右，每一个盘要有一个/efi的主分区，用来当作引导，gpt的磁盘格式不限制主分区的数目，也不先限制分区不超过2T。
###解决（win10和ubuntu在不同磁盘上）
这样，先用PE工具进去系统，使用diskgenius将SSD和HDD都变成UEFI格式，其中SSD要有efi和msr分区，HDD不要。之后在ssd上使用制作成uefi+gpt的win10系统盘装程序到C盘，之后再另一个HDD上分配需要的DEF盘给win10。  

再之后就可以使用制作成uefi+gpt的ubuntu系统装ubuntu，不过他的uefi的分区需要自己在装的时候给，而不是通过diskgenuis快速分区。
给ubuntu的分区是这样的：

- /efi    1G
- /swap	  16g
- /home	 160g
- /	     160G

装完之后，需要使用ubuntu，就开机的时候按F11，进去HDD的盘（因为HDD盘的efi分区只有一个ubuntu系统，其他的是win10的资料盘等）

最后，总算大功告成。
两个系统刚装完都会很卡，分辨率很奇怪，是因为显卡驱动没装上。


###解决（win10和ubuntu都在固态上）
重新安装了一次ubuntu，这次装的是16.04，并且装在固态上

- /efi   1G     固态
- /swap	8g      固态    
- /home	360g   机械
- /	    91G    固态

相当于固态这一个硬盘上有两个efi分区，但是启动是优先ubuntu的grub2
这样是最佳的方案。

但是好像装机还是和主板有关系，用贺凯的华硕主板装机就没什么问题，也很流畅，但是用这个msi主板装ubuntu就一大堆问题。（后来发现并不是，而是显卡驱动不推荐用命令行在线装，推荐的是使用下载下来之后再装）

现在我的问题是我的系统进ubuntu会很卡。有时候只有终端，没有图形界面
最后发现问题是，显卡驱动的安装方式有问题，不推荐使用三行代码自动装，而是自己下好安装包安装。

之后就是一个很完美的系统了。
现在Ubuntu系统盘里面有：ubuntu16.04系统，nvidia-smi 2070的显卡驱动、google chrome的deb安装包。这一套完成就很完美了。

对了，装机的u盘制作推荐refus，最小巧

