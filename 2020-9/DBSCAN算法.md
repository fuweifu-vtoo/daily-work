## DBSCAN算法

DBSCAN（Density-Based Spatial Clustering of Applications with Noise，具有噪声的基于密度的聚类方法）是一种聚类算法：是一种**基于密度**的空间聚类算法。 该算法将 具有足够密度的区域 划分为簇，并在具有噪声的空间数据库中发现 任意形状的簇 ，它将簇定义为密度相连的点的最大集合。

基于密度这点有什么好处呢，我们知道kmeans聚类算法只能处理球形的簇，也就是一个聚成实心的团（这是因为算法本身计算平均距离的局限）。但往往现实中还会有各种形状，比如下面两张图，环形和不规则形，这个时候，那些传统的聚类算法显然就悲剧了。于是就思考，样本密度大的成一类呗。这就是DBSCAN聚类算法。

![img](https://img-blog.csdn.net/20180718113640485?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YWNoYV9f/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

看一下DBSCAN的聚类结果，如下：

![img](https://img-blog.csdn.net/20180725173312977?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YWNoYV9f/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如果minPoints参数设置再大一点，那么这个笑脸可能会更好看。没有颜色标注的就是圈不到的样本点，也就是离群点，DBSCAN聚类算法在检测离群点的任务上也有较好的效果。如果是传统的Kmeans聚类，效果如下：

![img](https://img-blog.csdn.net/20180725174037783?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YWNoYV9f/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**DBSCAN聚类过程**

 **1.DBSCAN发现簇的过程**

​    初始，给定数据集D中所有对象都被标记为“unvisited”，DBSCAN随机选择一个未访问的对象p，标记p为“visited”，并检查p的![img](https://images0.cnblogs.com/blog2015/343064/201503/151610118706973.png)是否至少包含MinPts个对象。如果不是，则p被标记为噪声点。否则为p创建一个新的簇C，并且把p的![img](https://images0.cnblogs.com/blog2015/343064/201503/151640290117554.png)中所有对象都放在候选集合N中。DBSCAN迭代地把N中不属于其他簇的对象添加到C中。在此过程中，对应N中标记为“unvisited”的对象![img](https://images0.cnblogs.com/blog2015/343064/201503/151614195279100.png),DBSCAN把它标记为“visited”，并且检查它的![img](https://images0.cnblogs.com/blog2015/343064/201503/151610417771110.png)，如果![img](https://images0.cnblogs.com/blog2015/343064/201503/151617385429133.png)的![img](https://images0.cnblogs.com/blog2015/343064/201503/151617519801673.png)至少包含MinPts个对象，则![img](https://images0.cnblogs.com/blog2015/343064/201503/151618145587804.png)的![img](https://images0.cnblogs.com/blog2015/343064/201503/151611083868173.png)中的对象都被添加到N中。DBSCAN继续添加对象到C，直到C不能扩展，即直到N为空。此时簇C完成生成，输出。

   为了找到下一个簇，DBSCAN从剩下的对象中随机选择一个未访问过的对象。聚类过程继续，直到所有对象都被访问。

**2.DBSCAN聚类算法流程**

 ![img](https://images0.cnblogs.com/blog2015/343064/201503/151620017451283.png)