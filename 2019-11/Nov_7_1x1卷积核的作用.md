Nov_7_1x1卷积核的作用
=====

1. 降维/升维,以及降低计算量

2. 增加非线性:1*1卷积核，可以在保持feature map尺度不变的（即不损失分辨率）的前提下大幅增加非线性特性（利用后接的非线性激活函数），把网络做的很deep。

3. 跨通道信息交互（channal 的变换:使用1x1卷积核，实现降维和升维的操作其实就是channel间信息的线性组合变化，3x3，64channels的卷积核后面添加一个1x1，28channels的卷积核，就变成了3x3，28channels的卷积核，原来的64个channels就可以理解为跨通道线性组合变成了28channels，这就是通道间的信息交互.



用到1x1卷积的网络(举例子)
---
1. mobilenet
2. resnet
3. inception network
