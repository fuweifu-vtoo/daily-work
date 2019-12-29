# Sep_12_unpaired and paired image-to-image translation学习

------

1. unpaired and paired image-to-image translation 实际上就是对应的pix2pix和cycleGAN。
2. 如果想做图像翻译，最直接的方法是设计一个CNN网络，直接建立输入-输出的映射，可是这样做会带来一个问题：生成图像质量不清晰。用语义分割为例子，语义分割图的每个标签比如“汽车”可能对应不同样式，颜色的汽车。那模型学习到的会是所有不同汽车的评均，这样会造成模糊。例如直接用L1 Loss来学习得到的结果，相比于Ground truth，会模糊很严重。
3. 如何消除模糊，有一个办法是加入GAN的Loss去惩罚模型，也就是用pix2pix。pix2pix模型和CGAN有所不同，但它是一个CGAN，只不过输入只有一个，这个输入就是条件信息。原始的CGAN需要输入随机噪声，以及条件。