##Oct_11_RPN网络


### RPN

1. faster-RCNN中的RPN网络是为了替换之前的selective search，直接用CNN得到最后的结果，将找候选框的工作也交给神经网络，RPN负责初步寻找proposal的位置；

2. faster rcnn ： ![](http://markdown.vtoo.pro/2019Decfaster_rcnn1.jpg)

3. faster rcnn 的几个关键步骤：
	- 进行RPN操作，一个3x3的卷积后面分两路，分别连接一个1x1的卷积用来进行分类和回归操作；
	- ROI Pooling层之后，连接两个1024层的全连接网络层，然后分两个支路，连接对应的分类层和回归层；

4. 首先提一下anchor这个概念，把这个feature map上的每一个点映射回原图，得到这些点的坐标，然后着这些点周围取提前设定好的区域(在faster rcnn中一个点会有9个anchor,也就是9个预先设定好的区域)，这些选好的区域可以用来训练RPN。

5. anchor是预先设定好的尺寸大小，可以k-means聚类之后自己设定；

6. rpn网络会对所有候选框采样，training阶段，RPN网络提出了2000左右的proposals，首先会计算每个proposal和gt之间的iou，通过人为的设定一个IoU阈值（通常为0.5），把这些Proposals分为正样本（前景）和负样本（背景），并对这些正负样本采样，使得他们之间的比例尽量满足（1:3，二者总数量通常为128），之后这些proposals（128个）被送入到Roi Pooling，最后进行类别分类和box回归。inference阶段，RPN网络提出了300左右的proposals，这些proposals被送入到Fast R-CNN结构中，和training阶段不同的是，inference阶段没有办法对这些proposals采样（inference阶段肯定不知道gt的，也就没法计算iou），所以他们直接进入Roi Pooling，之后进行类别分类和box回归。

7. 等待补充，上下两个分支哪个是分类，哪个是回归？【最好能看源码】

### RPN网络的缺点:
---
1. RPN也有缺点(无法检测过小的物体)，最大的问题就是对小物体检测效果很差，假设输入为512*512，经过网络后得到的feature map是32*32，那么feature map上的一个点就要负责周围至少是16*16的一个区域的特征表达，那对于在原图上很小的物体它的特征就难以得到充分的表示，因此检测效果比较差。

2. 为了解决rpn网络无法解决小目标的问题，可以引入FPN网络；