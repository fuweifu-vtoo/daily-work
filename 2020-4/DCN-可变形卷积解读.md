## DCN-可变形卷积解读

1. 全称：Deformable Convolutional Networks（可变形卷积网络）

2. DCN卷积的优点：
	- 可以适应物体的尺度
	- 可以适应物体的形状  
	- 标准卷积（a）中的固定感受野和可变形卷积（b）中的自适应感受野 

3. 可变形卷积论文提出了两个新模块来提升CNNs的形变建模能力，称为“deformable convolution”和“deformable ROI pooling”；

4. 基于一个平行网络学习offset（偏移），使得卷积核在input feature map的采样点发生偏移，集中于我们感兴趣的区域或者目标。通过研究发现，标准卷积中的规则格点采样是导致网络难以适应几何形变的“罪魁祸首”，为了削弱这个限制，对卷积核中每个采样点的位置都增加了一个偏移变量，可以实现在当前位置附近随意采样而不局限于之前的规则格点。

5. 可变形卷积 并不是对kernel学习offset而是对feature的每个位置学习一个offset；

6. 参考[Deformable Convolution 关于可变形卷积](https://zhuanlan.zhihu.com/p/62661196)

7. 参考[Deformable Convolutional Networks-v1-v2(可变形卷积网络)](https://www.cnblogs.com/ranjiewen/p/10040410.html)
	
