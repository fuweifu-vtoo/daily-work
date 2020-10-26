## BN,GN,LN,IN的介绍及常考点

1. BN层的参数量：设特征图大小为：N*H*W*C，则BN层参数的个数是：C，一个特征图内会共享beta和gamma参数，所以只有C个参数，其是算N*H*W的均值；

2. 公式：![](http://markdown.vtoo.pro/2019Dec20181225231741578.png)

3. GN和BN性能对比：![](http://markdown.vtoo.pro/2019Dec101604sr3tllljtja6t5zu.jpg)

4. BN,GN,LN,IN的介绍:![](http://markdown.vtoo.pro/2019Dec20181117174408398.png)

5. BN，LN，IN，GN从学术化上解释差异：
	- BatchNorm：batch方向做归一化，算N*H*W的均值
	- LayerNorm：channel方向做归一化，算C*H*W的均值
	- InstanceNorm：一个channel内做归一化，算H*W的均值
	- GroupNorm：将channel方向分group，然后每个group内做归一化，算(C//G)*H*W的均值
	
6. BN存在的问题：BN 需要用到足够大的批大小（例如，每个工作站采用 32 的批量大小）。一个小批量会导致估算批统计不准确，减小 BN 的批大小会极大地增加模型错误率。加大批大小又会导致内存不够用。

7. GN 把通道分为组，并计算每一组之内的均值和方差，以进行归一化。GN 的计算与批量大小无关，其精度也在各种批量大小下保持稳定。

8. GN相当于在归一化的道路中避开了batchsize，即避开归一化对batch size依赖；

9. BN的batchsize在训练，验证，测试这三个阶段经常是不一样的，这也导致了分布不一样；

10. LN，IN也在一定程度上避开了归一化对batch size依赖，但是其在视觉任务的效果不明显，而GN是LN和IN的综合；

11. 而GN介于LN和IN之间，其首先将channel分为许多组（group），对每一组做归一化，及先将feature的维度由[N, C, H, W]reshape为[N, G，C//G , H, W]，归一化的维度为[C//G , H, W]；

12. [GN在tensorflow中的代码实现](https://blog.csdn.net/u014380165/article/details/79810040)；

13. 异曲同工的还有，在图像配准中提取点云全局特征的Context Normalization；
