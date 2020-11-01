#Sep_22_CUDA_VISIBLE_DEVICES_sync_bn_inplace_abn_apex_fp16混合精度训练_梯度累积


1. CUDA_VISIBLE_DEVICES是用于指定GPU的，使用终端命令行运行的 GPU 指定方式：`CUDA_VISIBLE_DEVICES=1 python python_script.py`，详情可以参考这个[https://www.aiuai.cn/aifarm483.html](https://www.aiuai.cn/aifarm483.html)

2. sync_bn(synchronize batch normalization):在一般的视觉问题上，单卡的batchsize其实已经够大，没必要把所有卡上的都统计一遍。然而到了现在的检测或者分割问题上，有些大模型模型单卡只能bz=1，这样的话BN完全无法发挥作用，所以我们需要在更多的卡上同步bn。即是单卡bn还是多卡bn的问题。

3. 节省显存的一个小技巧：尽可能使用inplace操作， 比如relu 可以使用 inplace=True。

4. inplace_abn:先进的深度网络中，大多数重复使用(BN+激活层)组合。而现有的深度学习框架对此的内存优化策略不佳。论文提出了INPLACE-ABN层代替BN+激活层，相比于标准的BN+激活层节省了50%的内存,节省了50%的存储空间，却只增加少许计算量。详细见[网络优化-- (INPLACE-ABN)](https://blog.csdn.net/u011974639/article/details/79545363)

5. 对于inplace_abn，光会用还不行，还要会做理论推导，inplace_abn本身是作为一篇文章发布的。

6. Inplace ABNSync：inplace_abn也可以做并行，变成Inplace ABNSync，详细见[Inplace ABNSync 与 pytorch GPU多卡并行的一点坑](https://blog.csdn.net/pku_Coder/article/details/85111082)

7. pytorch的两个问题：
	- 无法混合精度训练，难以收敛
	- sync_bn的问题

8. apex解决了pytorch上面的两个问题。
	- 例如，apex.FP16_Optimizer目前能够完整的复现fp32的训练结果，并且速度要更快，官方文档里用了“"highway" for FP16 training”
	- apex内置了SyncBatchNorm。
	>apex.parallel.SyncBatchNorm extends torch.nn.modules.batchnorm._BatchNorm to support synchronized BN. It reduces stats across processes during multiprocess distributed data parallel training. Synchronous Batch Normalization has been used in cases where only very small number of mini-batch could be fit on each GPU. All-reduced stats boost the effective batch size for sync BN layer to be the total number of mini-batches across all processes. It has improved the converged accuracy in some of our research models.

9. 梯度累积(gradient accumulation)，用于解决显存太小的问题：
```
for i,(images,target) in enumerate(train_loader):
    # 1. input output
    images = images.cuda(non_blocking=True)
    target = torch.from_numpy(np.array(target)).float().cuda(non_blocking=True)
    outputs = model(images)
    loss = criterion(outputs,target)

    # 2.1 loss regularization
    loss = loss/accumulation_steps
    # 2.2 back propagation
    loss.backward()
    # 3. update parameters of net
    if((i+1)%accumulation_steps)==0:
        # optimizer the net
        optimizer.step()        # update parameters of net
        optimizer.zero_grad()   # reset gradient
```

	梯度累加则实现了batchsize的变相扩大，如果accumulation_steps为8，则batchsize '变相' 扩大了8倍，是我们这种乞丐实验室解决显存受限的一个不错的trick，使用时需要注意，学习率也要适当放大,适当调低BN自己的momentum参数(其默认为0.1)
    详情还是参考知乎的回答：[PyTorch中在反向传播前为什么要手动将梯度清零？](https://www.zhihu.com/question/303070254)
