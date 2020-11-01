# June 25

- 一个神经网络的四个关键是：网络本身、损失函数、优化器、激活函数。
- Conv2d和Convtranspose2d的内部操作和参数.
- 搭建网络工程，我觉得现在最困难的是dataset这部分。
- 分割的很多label都被转存为mat或者npy格式的文件。

---------------
##遇到的问题记录下来
- [独热编码的必要性和转换方式](https://blog.csdn.net/weixin_42587745/article/details/82831172)
- [Missing key(s) in state_dict: Unexpected key(s) in state_dict:](https://blog.csdn.net/kaixinjiuxing666/article/details/85115077)这是因为训练的时候用了nn.DataParallel(fcn_model, device_ids=num_gpu)，而在inference的时候没有使用
- [detach的作用](https://blog.csdn.net/z_lbj/article/details/79604104):	去除variable中梯度的关联和反传。
- argmax与one hot编码结合使用，在解码的地方很有帮助。[np.argmax的用法](https://www.cnblogs.com/touch-skyer/p/8509217.html)

---------
- 如果对于batchsize=1的一张图片中的像素点，有好几类或者包含所有类的话，可以让batchsize=1，但是如果这一张图中的像素点类别很少，就会导致每一次训练会为正在训练的类别修正收敛方向，导致最终不收敛。
- 使用nn.CrossEntropyLoss会自动加上Softmax层，不用自己手动加。而使用nn.BCELoss需要在该层前面加上Sigmoid函数。CrossEntropyLoss可以用于二分类，也可以用于多分类，BCELoss只用于二分类问题，但如果用onehot形式的label，某个像素点如果只属于某一类，还要将sigmoid改为softmax。
- 有两种对label的处理方式：一：将label转为16\*H\*W 的 one hot格式，使用sigmoid之后用BCEloss二：将label转为 单通道的 H\*W 的格式，里面是0-15的数字标签，之后使用CrossEntropyLoss。