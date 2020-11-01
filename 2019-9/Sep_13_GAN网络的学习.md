# Sep_13_GAN网络的学习

------

1. GAN网络一般都有两个优化器，损失函数类型有时候一个,有时多个，但在训练的时候经常是多个，每一个都会进行backward()，之后再optimizer.step()。
2. 训练GAN的步骤：
	- 训练判别器
    	1.固定生成器的梯度(detach)，不让它训练，但要生成假图。
    	2.对真值图片，判别器尽可能输出1。
    	3.对生成器生成的图片，判别器要尽可能输出0。
    - 训练生成器
    	1.固定判别器的梯度(只调用optimizer_g)，但是要让它继续判别，告诉生成器哪里不好，所以。
    	2.生成器生成的图片要尽可能让判别器输出1。
    - 返回第一步，循环交替
    - 一种策略是：每一个batch训练一次判别器，每5个batch训练一次生成器。

3. 经典的GAN网络的学习，参考仓库[PyTorch-GAN](https://github.com/eriklindernoren/PyTorch-GAN)：
	- GAN
	- CGAN(条件gan)
	- DCGAN
	- pix2pix(用于pair image-to-image translation)
	- cycleGAN(起初用在style transfer，也用于unpair image-to-image translation)
	- SRGAN(用在超分辨率重建)，SRGAN的特点在于，它的生成器SRResNet本身就可以作为一个超分辨率的网络，训练的时候也有pretrain操作单独训练SRResNet，但是它加入了对抗的思想，用一个判别器继续教生成器如何去优化生成器本身，步骤是先训练判别器，之后训练(优化)生成器。文章中的实验结果表明，只用基于均方误差MSELOSS的损失函数训练的SRResNet(不加GAN的情况)，得到了结果具有很高的峰值信噪比，但是会丢失一些高频部分细节，图像比较平滑。而SRGAN得到的结果则有更好的视觉效果。其中，又对内容损失分别设置成基于均方误差、基于VGG模型低层特征和基于VGG模型高层特征三种情况作了比较，在基于均方误差的时候表现最差，基于VGG模型高层特征比基于VGG模型低层特征的内容损失能生成更好的纹理细节。
	- WGAN(散度gan)
	- SAGAN(自注意力gan)
	- LSGAN(最小二乘gan)

4. GAN的本质是生成网络，是基于对抗的生成网络。而风格迁移也是生成网络，但不是基于对抗的。