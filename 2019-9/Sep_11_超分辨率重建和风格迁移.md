## Sep_11_超分辨率重建和风格迁移

------
###超分辨率重建
1.仓库[pytrch/example]()中的super_resolution中使用的模型是ESPCN，即是["Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network" - Shi et al. ](https://arxiv.org/abs/1609.05158)。它会把输入的图片先resize到原来的1/upscale_factor，之后和原图形成一对label。代码中使用的数据集是BSDS300。很快就可以训练完。
2.github上仓库[super-resolution](https://github.com/icpm/super-resolution.git)有基本的超分辨率的网络，除了srcnn我感觉有点问题，其他都还好。查看的顺序：
	- SRCNN(Learning a Deep Convolutional Network for Image Super-Resolution, ECCV2014)
    - FSRCNN(Accelerating the Super-Resolution Convolutional Neural Network, ECCV2016)
    - ESPCN(SubPixelCNN)(Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network, CVPR2016)
    - VDSR(Accurate Image Super-Resolution Using Very Deep Convolutional Networks, CVPR2016)
    - DRCN(Deeply-Recursive Convolutional Network for Image Super-Resolution, CVPR2016)
    - SRDenseNet(Image Super-Resolution Using Dense Skip Connections, ICCV2017最佳)(这个github上仓库没有)
    - SRGAN(SRResNet)(Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network, CVPR2017)
    - EDSR(Enhanced Deep Residual Networks for Single Image Super-Resolution, CVPRW2017)
    - DBPN(cvpr2018)
3.看以上论文的时候，看知乎的一篇文章来辅助[从SRCNN到EDSR，总结深度学习端到端超分辨率方法发展历程](https://zhuanlan.zhihu.com/p/31664818)。
4.SR任务中都是只处理图像YCbCr中的Y通道即可，只有一层，所以看着像灰度图。另外两个通道用简单的上采样方法即可。
5.SRGAN(用在超分辨率重建)，SRGAN的特点在于，它的生成器SRResNet本身就可以作为一个超分辨率的网络，训练的时候也有pretrain操作单独训练SRResNet，但是它加入了对抗的思想，用一个判别器继续教生成器如何去优化生成器本身，步骤是先训练判别器，之后训练(优化)生成器。文章中的实验结果表明，只用基于均方误差MSELOSS的损失函数训练的SRResNet(不加GAN的情况)，得到了结果具有很高的峰值信噪比，但是会丢失一些高频部分细节，图像比较平滑。而SRGAN得到的结果则有更好的视觉效果。其中，又对内容损失分别设置成基于均方误差、基于VGG模型低层特征和基于VGG模型高层特征三种情况作了比较，在基于均方误差的时候表现最差，基于VGG模型高层特征比基于VGG模型低层特征的内容损失能生成更好的纹理细节。
6.EDSR最有意义的模型性能提升是去除掉了SRResNet多余的模块，从而可以扩大模型的尺寸来提升结果质量。例如去除了BN，有分析BN在超分辨率任务上表现不好。也加深了网络。

-----
###风格迁移
1.仓库[pytrch/example]()中的fast_neural_style中使用的backbone是vgg，毕竟vgg的拿手戏是风格迁移。并且它是属于[固定风格，任意内容的风格迁移]()。参考的是论文[Perceptual Losses for Real-Time Style Transfer and Super-Resolution](),改进是使用了Instance Normalization,代码中使用的数据集是coco2014。一次要训练很久,估计要一天。

2.还有两种风格迁移类型：[固定风格固定内容的普通风格迁移，A Neural Algorithm of Artistic Style](A Neural Algorithm of Artistic Style)和[任意风格任意内容的极速风格迁移，Meta Networks for Neural Style Transfer](Meta Networks for Neural Style Transfer)。

3.在github上查看到了一个仓库,[PytorchNeuralStyleTransfer](https://github.com/leongatys/PytorchNeuralStyleTransfer)，应该是固定风格固定内容的普通风格迁移，CVPR2016。他们的论文是[Image Style Transfer Using Convolutional Neural Networks.](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Gatys_Image_Style_Transfer_CVPR_2016_paper.html)想知道和A Neural Algorithm of Artistic Style的区别在哪里。一次训练很快，因为是使用的VGG19预训练模型。

5.github上的一个仓库[StyleTransferTrilogy](https://github.com/CortexFoundation/StyleTransferTrilogy.git),讲述了风格迁移三部曲，也是一个很好的工程。另外，里面包含了[任意风格任意内容的极速风格迁移]()，需要两个数据集，内容数据集采用的是coco2014，风格数据集采用的是[wikiart](https://www.kaggle.com/c/painter-by-numbers/data)。

6.风格迁移一般是采用vgg来做，采用resnet来做会让风格不是很明显，但是在量子位的一篇文章[谁说只有VGG才能做风格迁移，ResNet也可以！答案就在对抗攻击中](https://zhuanlan.zhihu.com/p/71684076)提到，并不是因为resnet不能用于风格迁移，而是需要一个比较robust的resnet 才可以做到风格迁移，并且比vgg在细粒度上效果更好。