Sep_31_CNN网络的Params和FLOPs的计算
===

1. 实时语义分割中的评价指标通常有:mIoU,FPS,Params,FLOPS,Memory等.

2. 其中MIOU和FPS可以通过通用代码计算出.

3. Memory指的是模型占内存的大小.

4. Params指的是模型中参数的个数,计算通常分为卷积层和全连接层:
	- 对于一个卷积层,参数个数为:![](./images/params.jpg)
	- 对于全连接层,输入维度I,输出维度O,参数个数为I * O + O,其中后面的O包括了bias.

5. FLOPS指的是浮点数运算（float point operation）,是一个常用的衡量指标.计算通常分为卷积层和全连接层:
	- 对于一个卷积层,![](./images/flops.jpg)
	- 对于全连接层,输入维度I,输出维度O, Flops = I*O+I*O-O + O,其中后面的I*O-O代表加法运算,因为N个数相加只需要加N-1次,后面的 + O代表bias.
	- 注意:H和W是输出feature map的特征图大小，不是输入的特征图大小，输出的H和W与输入的H和W之间存在着stride的关系；

6. Flops的计算这里计算的是MAC（乘法加法操作），在一些论文里是仅仅考虑乘法操作.

7. 可以发现,计算卷积层的FLOPs中的乘法操作,只需在parameters的基础上再乘以输出feature map的大小即可.

8. 全文参考[关于CNN网络的FLOPs的计算](https://blog.csdn.net/shwan_ma/article/details/84924142),[深度学习中parameters个数和FLOPS计算](https://blog.csdn.net/qq_36653505/article/details/86700885)

9. 发现github上的 torchsummaryX 可以很好的计算params和GFlops,直接一句话'summary(your_model, torch.zeros((1, 3, 224, 224)))'就可以做到.其中mult-add代表的就是FLOPS.

