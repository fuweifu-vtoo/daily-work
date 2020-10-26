# 2D-3D registration

1. p3p是一种算法，是用于求解PNP问题算法中的一种；RANSAC算法是一种迭代算法，即随机采样一致算法，专门用于去除‘离群点’的算法；

2. 拟合平面时，如果有离群点，就采用RANSAC算法，没有离群点，就采用最小二乘法；

3. pointnet，这篇讲的非常详细:[https://zhuanlan.zhihu.com/p/86331508]();pointnet++,看这篇:[https://zhuanlan.zhihu.com/p/88238420]()

4. pointnet中的"mlp"代表"multi-layer perceptron"（多层感知机）。其中，shared mlp是通过共享权重的1x1卷积实现的，因为是一维卷积，卷积核大小也可以看做是1x(in_channel);mlp还可以使用全连接来实现，例如pointnet最后的一个mlp是使用的全连接层得到score的；（在论文中，mlp和shared mlp一个意思）；

5. pointnet中的两点：针对点云无序性——采用Maxpooling作为对称函数；针对刚体变化——对齐网络T-net网络，即空间变换网络；pointnet的具体实现还是要参照着论文去看，很多代码使用max直接代替了maxpooling；

4. pointnet++相对于pointnet而言，相当于考虑了PointNet无法获得局部特征，由三个部分组成：sampler layer、grouping layer、pointnet layer；分别负责选取中心点、根据中心点划分区域、对区域提取局部特征；

5. edgeconv也可以做到提取局部特征的效果；点云分类中常用的数据集:[Modelnet40]()、[ShapeNet]();

6. 受PointNet利用了多层感知器（MLP）。 由于MLP在不同的点上进行操作，这与卷积网络或密集网络不同，因此，合并来自其他感知器（即上下文）的信息对于使其正常工作是必不可少的。

7.  PointNet和Context Normalization之间的区别在于如何做到合并来自其他感知器（即上下文）的信息。

8. Context Normalization：我们将对应关系中每个感知器的输出归一化，但对每个图像对则分别进行归一化。 这允许分布特征图以对场景几何形状和摄像机运动进行编码，将上下文信息嵌入与上下文无关的MLP中。

9. Context Normalization：相比之下，其他标准化技术则主要集中在收敛速度上，而对网络的运行方式影响很小。 批量归一化[14]通过一个小批量对每个神经元的输入进行归一化，从而使其在训练时遵循一致的分布。 层归一化[1]将此操作转换为通道，因此与每个神经元的观察次数无关。

10. [面向非结构化环境下的非特定类别图像的配准算法，医学影像中的配准算法暂不考虑](https://www.zhihu.com/question/32066833).

11. pointnet中获得全局特征的做法是：max pooling，它是在堆叠了block之后的特征做最大池化获得全局特征； 而Context Normalization也可以获得全局特征，它是在每个block之中操作，而获得全局特征；

12. 为了解决图像的旋转不变性问题，二维中经常采用的是STN（空间变换网络），详细见[https://blog.csdn.net/u011961856/article/details/77920970](https://blog.csdn.net/u011961856/article/details/77920970)，即采用需要学习的仿射变换对feature处理,而在pointnet中为了解决旋转不变形，采用的T-NET中需要学习的3x3的矩阵；

13. pointnet中的MLP是用1x1卷积实现的；

14. 仿射变换看：[https://www.cnblogs.com/shine-lee/p/10950963.html](https://www.cnblogs.com/shine-lee/p/10950963.html)，仿射变换是一种二维坐标到二维坐标之间的线性变换，它保持了二维图形的“平直性，仿射变换矩阵有6个自由度：![](http://markdown.vtoo.pro/2019Decb975bdb57a9e8910364d8f3d8b1a433.png)

15. 透视变换看：[https://blog.csdn.net/flyyufenfei/article/details/80208361](https://blog.csdn.net/flyyufenfei/article/details/80208361),透视变换将二维（x,y）到三维(X,Y,Z)，再到另一个二维(x’,y’)空间的映射：![](http://markdown.vtoo.pro/2019Decc17036a7c01c379ec6438ae3aad045b.png)．透视变换矩阵好像也是8个自由度？

16. 单应性矩阵看：[https://blog.csdn.net/lyhbkz/article/details/82254893](https://blog.csdn.net/lyhbkz/article/details/82254893)，单应性矩阵可以看做是相机内参矩阵和外参矩阵的乘积；用来描述物体在世界坐标系和像素坐标系之间的位置映射关系；单应性矩阵中有8个自由度；

17. 单应性矩阵: 感觉单应性矩阵说是世界坐标系和像素坐标系之间的位置映射关系，但是单应性矩阵做的还是透视变换的事情，且在单应性矩阵中的输入的Z恒等于1；单应性矩阵：![](http://markdown.vtoo.pro/2019Dece5ec7daa241ebdc8888cd03fe31f178.png)

18. 单应性变换矩阵和透视变换，逆透视变换的关系；[https://blog.csdn.net/dataningwei/article/details/59108376](https://blog.csdn.net/dataningwei/article/details/59108376)

19. 相机外参矩阵有6个自由度，其中旋转矩阵3个自由度，平移矩阵3个自由度，外参矩阵是一个3x4的矩阵：![](http://markdown.vtoo.pro/2019Dec7f3a51a92f0eba7c6d9da49cfb7db67.png);

20. 相机内参矩阵K是一个3x3的矩阵：![](http://markdown.vtoo.pro/2019Dec967f3ee199c2234190321dbee6f40ae.png),其中fx,fy代表焦距，x0,y0代表主点偏移，s代表轴倾斜，轴倾斜会导致投影图像的形变；第二个等式右边三个矩阵依次是：2D平移、2D缩放、2D切变；

21. 通过2D-3D配准，求解相机姿态，使用pnp算法；

22. 通过2D-2D配准，求解相机姿态，使用经典的8点算法；SLAM十四讲之：2D-2D配准[https://www.cnblogs.com/ChrisCoder/p/10035562.html](https://www.cnblogs.com/ChrisCoder/p/10035562.html)，里面有好多专业术语和知识，强烈建议要去看！；

23. 2D-3D配准和2D-2D配准的最终目的，都是为了求解相机姿态；而