Nov_20_resnetV1和resnetV2
====

1. resnetv1:![](./images/resnetv1.png).

2. ResnetV2采用的是“pre-activation(预激活)”的方式![](./images/resnetv2.png)预激活的影响具有两个方面:
	- 第一，优化变得更加简单(与原始ResNet相比)。
	- 第二，在预激活中使用BN能够提高模型的正则化.

3. 平时常用的resnet指的是resnetV1.