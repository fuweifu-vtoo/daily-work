## 轻量化网络EfficientNet和轻量化目标检测网络EfficientDet

### 问题描述

1. 一般对卷积网络的扩展，是通过调整：图像大小，网络深度，网络宽度(即channel数)来实现的，大部门都只针对其中的一个进行调整，因为限于计算资源和人工调参等因素，很少是三个综合调整的；

2. efficientnet就是综合三个因素进行调整，问题描述为：如何平衡分辨率，深度和宽度这三个维度，实现卷积网络在准确率和效率上的优化？

3. efficientnet给出的解决方案时一种模型复合缩放方法：![](http://markdown.vtoo.pro/2019Deceffi.png)图a是基线网络。图b，c，d是这三个网络分别对宽度，分辨率和深度进行扩展，图e是efficientnet的主要思想，综合平衡分辨率，深度和宽度这三个维度进行复合扩展；

4. 知道复合扩展的方法之后，就是采用MnasNet去搜索最佳结构，设置约束等之类的；

### EfficientNet

1. EfficientNet-B0是用MnasNet的方法搜出来的，利用这个作为baseline来联合调整深度、宽度以及分辨率，从而得到Efficient-B1 到 B7； 
  
2. Efficient-B1 到 B7 是扩展基线模型后得到的网络。 
  
3. EfficientNet 性能图：![](http://markdown.vtoo.pro/2019Decefficientnet.png) 
  
4. EfficientNet-B0是强化学习搜出来的网络架构，其效果比resnet要好，由此可见强化学习的上限相比人设计而言，可能更高。   

5. EfficientNet-B0的结构图：![](http://markdown.vtoo.pro/2019DecefficientnetB0.png)  
 
6. EfficientNet-B0和手工设计的网络(如resnet)有明显的几处区别：   
	- 在最开始降采样没有采用maxpooling，而是换成了stride为2的conv，猜测是为了减少信息丢失，尤其是对小模型来说，前期的底层特征提取更重要。   
	- 第一次降采样后的channel反而减少了！ 
	- 有很多stage都采用了5x5的MBconv:这是因为对于depthwise separable conv来说，5x5的计算量要比两个3x3的计算量要小。（坊间传闻large kernel is all your need.)    
	- 第四处区别是降采样后的特征图尺寸减半，但是channel没有扩大两倍。第6个stage特征图尺寸没变，但是channel也扩大了   
	- 以上都是手工设计很难搞定的和想到的！ 

7. 文章中指的商榷的一点：EfficientNet, AmobaNet, ResNet / DenseNet 的 training setting 是完全不一样的。AutoAugment 是一个非常强的 augementation，这套 learning rate schedule 也是针对 MBConv 专门设计的。去掉以后，用 ResNet 那一套 setting 去训练 EfficientNet 后 (120 epoch, 30 epoch decay by 1/10），b0 的 accuracy 从  76.7 直接掉到不到 74 （ResNet-34, MobilenetV2-1.3 的水平）。这么一比，在小模型的上优势就不是那么明显了。

8. 文章的一个novelty：不在于小模型上的 efficiency，而是告诉大家 MBConv 是可以被 scale 到 non-mobile settings 的。MBConv 自被提出以来就一直局限在 light-weight model，而在拼点的任务上，大家都还停留在之前的 ResBlock + SE 那一套。 EfficientNet 通过实验告诉了大家 MBConv 也可以做 large setting（印象中应该是第一个）。

9. 性能评价： efficientnet的性能远超其他网络，如resnet和resnext和senet等，无论在精确度还是速度上都是，EfficientNet-B7 取得了新的 SOTA 结果：84.4% top-1 / 97.1% top-5 准确率。EfficientNet-B1 的参数量远远小于 ResNet-152，两者的精度上差不多，但速度前者是后者的 5.7 倍。(所以以后用EfficientNet-B1或EfficientNet-B2即可，但是训练上不是按照resnet的训练策略，有专门的训练策略)

10. 迁移能力：尽管 EfficientNets 在 ImageNet 上性能优异，但要想更加有用，它们应当具备迁移到其他数据集的能力。谷歌研究人员在 8 个常用迁移学习数据集上评估了 EfficientNets，结果表明 EfficientNets 在其中的 5 个数据集上达到了当前最优的准确率，且参数量大大减少，这表明 EfficientNets 具备良好的迁移能力。

11. 参考：[如何评价谷歌大脑的EfficientNet](https://www.zhihu.com/question/326833457/answer/700322601)
12. 参考：[为什么EfficientNet号称最好的分类网络](http://www.360doc.com/content/19/0918/19/7669533_861835495.shtml)

### MBCONV

1. 全称：mobile inverted bottleneck convolution (MBConv)，它是EfficientNets模型家族的种子；其也是mobilenetV2中的主干卷积block；    

2.  MBConv 上一个 5x5 的 FLOPs 比两个 3x3 要小，即large kernel MBConv 会比 small kernel 更好。![](http://markdown.vtoo.pro/2019Dec55mbconv.png)  

3.  坊间传闻，对于MBConv而言，large kernel is all your need.


### EfficientDet

1. [谷歌最新目标检测器EfficientDet](https://baijiahao.baidu.com/s?id=1651514751262272881&wfr=spider&for=pc)

### NoisystudentEfficientNet/EfficientNet-L2

### FixEfficientNet

1. 最新的论文


