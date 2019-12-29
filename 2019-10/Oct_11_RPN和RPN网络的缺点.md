##Oct_11_RPN和rpn网络的缺点


###RPN
1. RPN:region proposal network,和SSD:Single-Shot detection，都是用于目标检测的机制。

2. 在大名鼎鼎的Faster-RCNN以及和它同一时期的工作YOLO之前，目标检测都是利用selective search找到proposal，然后再用CNN判断这个region proposal究竟是不是物体，是哪个物体，以及用CNN去优化这个框的位置，这种方法最典型的代表就是Faster-RCNN的前身，RCNN和Fast-RCNN。

3. faster-RCNN和YOLO解决的问题是省去了selective search，直接用CNN得到最后的结果，并且性能比之前的方法有很大提升（将找候选框的工作也交给神经网络）。

4. Faster-RCNN由RPN和Fast-RCNN组成，RPN负责寻找proposal，Fast-RCNN负责对RPN的结果进一步优化。

5. 其实RPN已经可以找到图片中每个物体的位置，如果更注重找到bbox的速度而不是精度的话可以只使用RPN。RPN是一个全卷积网络(FCN)，由于没有全连接层，所以可以输入任意分辨率的图像，经过网络后就得到一个feature map，然后利用这个feature map得到物体的位置和类别。

6. 首先提一下anchor这个概念，把这个feature map上的每一个点映射回原图，得到这些点的坐标，然后着这些点周围取提前设定好的区域(在faster rcnn中是9个anchor,也就是9个预先设定好的区域)，这些选好的区域可以用来训练RPN。

RPN网络的缺点:
---
1. RPN也有缺点(无法检测过小的物体)，最大的问题就是对小物体检测效果很差，假设输入为512*512，经过网络后得到的feature map是32*32，那么feature map上的一个点就要负责周围至少是16*16的一个区域的特征表达，那对于在原图上很小的物体它的特征就难以得到充分的表示，因此检测效果比较差。而SSD: Single Shot MultiBox Detector很好的解决了这个问题.(之后的Resnet-FPN的backbone解决了这个问题)


参考[RPN(Region Proposal Network) and SSD(Single Shot MultiBox Detector)](https://blog.csdn.net/shenziheng1/article/details/79653020)