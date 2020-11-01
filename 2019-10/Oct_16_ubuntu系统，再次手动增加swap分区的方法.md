##Oct_16_已安装ubuntu系统，再次手动增加swap分区的方法

###问题情景：
在某一次跑分割模型的时候，总是迭代至固定步数就被系统自动killed程序(`RuntimeError: DataLoader worker (pid 123456) is killed by signal`)，后通过nvidia-smi 查看显存使用情况，free -h 查看内纯使用情况，发现是在程序运行过程中，内纯先被用完，之后占用Swap分区，等Swap分区也被占用完之后，程序便会被系统kill.   

当时没有去想为什么内存会随着程序运行消耗殆尽，而是希望增加swap分区的容量，下面介绍如何增加swap分区。

###增加系统swap分区
1.查看硬盘使用情况：`df -lh`，找到可以为swap分区增加容量的磁盘，尽量选取机械硬盘。
2.创建swap文件：
```
mkdir /home/vtoo/swapfile

cd /home/vtoo/swapfile

sudo dd if=/dev/zero of=swap bs=1G count=16

```
3.转换swp文件:`sudo mkswap -f swap`
4.激活swap文件：`sudo swapon swap`或者卸载swap文件：`sudo swapoff swap`

可以用`free -h`查看增加后的swap分区的大小，但是这种swap的增加不是永久的，在系统重启后会失效。

###希望永久生效
1.如果要一直使用这个swap，要把它写入/etc/fstab文件中(直接双击打开，加入文件最后一行即可)：
`/home/roo/swapfile/swap none swap defaults 0 0`

###探讨程序运行中内存消耗越来越大的原因
之后发现是因为在程序中的三个变量导致：
`inputs_all, labels_all, predictions_all = [], [], []`
在后面的程序中会不断像三个lis中添加图像，以便用于评估指标，变量会一直存放在内存中，所以会越来越大。