1. faster rcnn 的四个loss组成： 
	- RPN层的两个损失：1.是否为前景背景的二分类交叉熵损失 2. bbox的第一次修正损失
	- 全连接层分类的两个损失：1.num_classes分类损失 2.第二次bbox修正损失

2. 介绍一下faster rcnn：![](./images/faster_rcnn1.jpg)展示了python版本中的VGG16模型中的faster_rcnn_test.pt的网络结构，可以清晰的看到该网络对于一副任意大小PxQ的图像，首先缩放至固定大小MxN，然后将MxN图像送入网络；而Conv layers中包含了13个conv层+13个relu层+4个pooling层；RPN网络首先经过3x3卷积，再分别生成positive anchors和对应bounding box regression偏移量，然后计算出proposals；而Roi Pooling层则利用proposals从feature maps中提取proposal feature送入后续全连接和softmax网络作classification（即分类proposal到底是什么object）

3. 经典的检测方法生成检测框都非常耗时,采用selective proposal，而faster rcnn是用的anchor技术，具体实现在rpn网络。

4. faster rcnn的源码需要看

5. faster rcnn 最佳参考学习文章：[一文读懂Faster RCNN](https://zhuanlan.zhihu.com/p/31426458)

6. [学一百遍都不算多的 Faster-RCNN](https://zhuanlan.zhihu.com/p/49897496)

7. 提到RPN网络，就不能不说anchors。所谓anchors，实际上就是一组由rpn/generate_anchors.py生成的矩形。直接运行作者demo中的generate_anchors.py可以得到9个anchor，即9个矩形。