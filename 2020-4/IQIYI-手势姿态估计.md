## 手势姿态估计学习

1. stacked hourglass network，参考[https://blog.csdn.net/wangzi371312/article/details/81174452](),即堆叠沙漏网络，专门用于人体姿态估计等这种关节点的预测中；

2. 为什么要用图卷积做关键点姿态估计：关节点之间是可以互相参考预测的，即知道双肩的位置后，可以更好的预测肘部节点，给出腰部和脚踝位置，又可以用于预测膝盖，这些关键点之间形成图；

3. GNN和发展而来的GCN；参考[图卷积神经网络](https://zhuanlan.zhihu.com/p/60962304),这里会提到一个图的聚类方法：谱聚类；

4. 浙大手势识别毕业论文中2D手势姿态估计中用到的网络和stacked hourglass 网络的结构很像，也是每个阶段都会参与计算loss，且上一阶段的输入M会继续到下一个阶段参与卷积训练；

5. CPM网络：[https://blog.csdn.net/zziahgf/article/details/79643752](https://blog.csdn.net/zziahgf/article/details/79643752),它能够同时学习图像和空间信息的特征表示；且不需要构建任何显式的关节点间关系模型（这个很重要，感觉CPM和stacked hourglass network很像，都有对中间阶段计算loss的操作）；

6. pointnet，这篇讲的非常详细:[https://zhuanlan.zhihu.com/p/86331508]();pointnet++,看这篇:[https://zhuanlan.zhihu.com/p/88238420]()

7. pointnet++相对于pointnet而言，相当于考虑了PointNet无法获得局部特征，由三个部分组成：sampler layer、grouping layer、pointnet layer；分别负责选取中心点、根据中心点划分区域、对区域提取局部特征；

8. edgeconv也可以做到提取局部特征的效果；点云分类中常用的数据集:[Modelnet40]()、[ShapeNet]();