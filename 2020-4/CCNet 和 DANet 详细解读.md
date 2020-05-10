## CCNet 和 DANet 详细解读

### Spatial Attention 和 Channel Attention

1. Self-attention结构自上而下分为三个分支，分别是query、key和value。计算时通常分为三步：
	- 第一步是将query和每个key进行相似度计算得到权重，常用的相似度函数有点积，拼接，感知机等；
	- 第二步一般是使用一个softmax函数对这些权重进行归一化；
	- 第三步将权重和相应的键值value进行加权求和得到最后的attention。

2. Spatial Attention：每个像素与另外每个元素都进行点乘操作，而向量的点乘几何意义为计算两个向量的相似度，两个向量越相似，它们点乘越大；

<center>
<img src="http://markdown.vtoo.pro/2019Decspatialattention.png" width="50%">
</center>

3. Channel Attention：让Channel与Channel之间进行点乘操作，计算Channel之间的相似性，在这里可以试着认为每张Channel Map代表了不同类别；

<center>
<img src="http://markdown.vtoo.pro/2019Decchannelattention.png" width="50%">
</center>

4. 沈哥说的话：所有提到attention的地方，attention加到网络中，提升性能意义不大。 他实际做的是特征的自动筛选和组合过程。 算法岗就是研究特征，所以重点就是attention分别特征权重具有可解释性，反过来，可以知道网络剪枝，或者，自动搜索最优子网络；

### DANet

1. DANet的结构图：

<center>       
<img src="http://markdown.vtoo.pro/2019DecDANet.jpg" width="80%" />    
</center>

### CCNet

1. CCNet用了巧妙的方法减少了参数量，DANet中，Attention Map计算的是所有像素与所有像素之间的相似性，空间复杂度为(HxW)x(HxW)，而本文采用了Criss-Cross思想，只计算每个像素与其同行同列即十字上的像素的相似性，通过进行循环(两次相同操作)，间接计算到每个像素与每个像素的相似性，将空间复杂度降为(HxW)x(H+W-1)；

<center>       
<img src="http://markdown.vtoo.pro/2019DecCCNet.jpg" width="60%" />    
</center>

2. CCNet整个网络的架构与DANet基本相同，只不过Attention模块有所不同。(但是同样也是q,k,v和Softmax)

<center>
<img src="http://markdown.vtoo.pro/2019Deccrisscross.png" width="60%">
</center>

3. 尽管纵横交错的注意力模块可以在水平和垂直方向捕捉长程上下文信息，像素和周围像素之间的关联仍然很稀疏。获取密集的上下文信息有助于语义分割。为了达到这个目的，在上面描述的交叉注意模型的基础上引入了递归的交叉注意。

<center>
<img src="http://markdown.vtoo.pro/2019DecCCNet1.png" width="90%">
</center>

4. 递归交叉注意模块有两个循环(R=2)，足够从所有像素中获取长期依赖关系，生成具有密集丰富上下文信息的新feature map 


## 参考
全文参考：[CCNet & DANet](https://segmentfault.com/a/1190000018271713?utm_source=tag-newest)