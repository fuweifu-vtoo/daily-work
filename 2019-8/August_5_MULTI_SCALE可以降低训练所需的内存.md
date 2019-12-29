##August 5

1. MULTI_SCALE有一个功能是，可以降低训练需要的内存，同样大小的显存能同事训练更多张图片。
2. python -m 中-m参数的作用。
3. dataloader中的 num_workers 参数可以肉眼可见加快训练速度。但不知道我为什么保存不了数据。
4. torch.distributed.launch --nproc_per_node=4 和local_rank是用于分布式训练，可以在多台计算机上训练。（在hrnet中出现了）