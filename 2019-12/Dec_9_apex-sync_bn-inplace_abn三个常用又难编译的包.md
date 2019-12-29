# Dec_9_apex-sync_bn-inplace_abn三个常用又难编译的包

## apex

1. github链接:[https://github.com/NVIDIA/apex.git](https://github.com/NVIDIA/apex.git)
2. 介绍: 在Pytorch中实现 混合精度训练 和 分布式训练
3. 编译成功环境配置:driver430 + cuda10.0.130 + pytorch1.1.0
```python
# Install **Apex**
$ git clone https://github.com/NVIDIA/apex
$ cd apex
$ pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```


## sync_bn

1. github链接:[https://github.com/zhanghang1989/PyTorch-Encoding.git](https://github.com/zhanghang1989/PyTorch-Encoding.git)
2. 介绍:实现了多gpu训练时BN的归一化问题,采用sync_bn
3. why sync_bn? [https://hangzhang.org/PyTorch-Encoding/notes/syncbn.html#why-synchronize-bn](https://hangzhang.org/PyTorch-Encoding/notes/syncbn.html#why-synchronize-bn)
4. how sync_bn? [https://hangzhang.org/PyTorch-Encoding/notes/syncbn.html#how-to-synchronize](https://hangzhang.org/PyTorch-Encoding/notes/syncbn.html#how-to-synchronize)
5. cvpr2017


## inplace_abn

1. github链接:[https://github.com/mapillary/inplace_abn.git](https://github.com/mapillary/inplace_abn.git)
2. 介绍: 将BN和非线性激活relu重新定义为一种操作，节省多达50％的内存.同时有sync_inplace_abn.
3. cvpr2018,In-Place Activated BatchNorm for Memory-Optimized Training of DNN
```python
# Install **Inplace-ABN**
$ git clone https://github.com/mapillary/inplace_abn.git
$ cd inplace_abn
$ python setup.py install
```

4. 编译成功编译成功环境配置:driver430 + cuda10.0.130 + pytorch1.1.0


## Sep_22_CUDA_VISIBLE_DEVICES_sync_bn_inplace_abn_apex_fp16混合精度训练_梯度累积

1. 可以返回去看==Sep_22_CUDA_VISIBLE_DEVICES_sync_bn_inplace_abn_apex_fp16混合精度训练_梯度累积==这一篇.