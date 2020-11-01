#KNN原理及代码实现及应用
（命运）总算是要对KNN下手了，之前的我一直知道KNN，但是老和k均值（k-means）搞混（记错名字），这次借着好久没有更新博客的机会，要巩固一下KNN，顺便将KNN的实现代码贴出来和贴出KNN在cifra-10数据集上的应用。
##KNN的原理
KNN的全称是**K Nearest Neighbor**（**K近邻算法**，也叫**最近邻算法**），它通过测量不同特征值之间的距离进行分类。  
常用的距离有两种：
   
- 欧几里得距离,L2 norm，即 L2 范数
- 曼哈顿距离,L1 norm,即 L1 范数

它的思路是：如果一个样本在特征空间中的k个最相似(即特征空间中最邻近)的样本中的大多数属于某一个类别，则该样本也属于这个类别，其中K通常是不大于20的整数。  
KNN算法中，所选择的邻居都是已经正确分类的对象（也就是我们称的训练集，实际上，KNN的训练集是不需要训练的）。该方法在固定类数决策上只依据最邻近的一个或者几个样本的类别来决定待分样本所属的类别。   
示意图如下：  
![Alt text](http://pdcknxeeg.bkt.clouddn.com/201810171.jpg)  
要决定绿色圆要被决定赋予哪个类，是红色三角形还是蓝色四方形？如果K=3，由于红色三角形所占比例为2/3，绿色圆将被赋予红色三角形那个类，如果K=5，由于蓝色四方形比例为3/5，因此绿色圆被赋予蓝色四方形类。
##KNN的算法描述
1）计算测试数据与各个训练数据之间的距离；

2）按照距离的递增关系进行排序；

3）选取距离最小的K个点；

4）确定前K个点所在类别的出现频率；

5）返回前K个点中出现频率最高的类别作为测试数据的预测分类。

##KNN的缺点 

- 需要大量空间存储所有已知实例，所有训练数据都是已知实例
- 当样本分布不均衡时，比如其中一类样本实例数量过多，占主导的时候，新的未知实例很容易被归类这个主导样本。
- 对于图像处理而言，计算速度取决于图像的大小（图像稍微一大，计算速度就很慢）

缺点这么多，这也是KNN算法用的非常少的原因，但是作为机器学习算法，还是有学习的必要。

##KNN的python实现 

```
#coding:utf-8
from numpy import *
import operator
import matplotlib.pyplot as plt

##给出训练数据以及对应的类别
def createDataSet():
    group = array([[1.0,2.0],[1.2,0.1],[0.1,1.4],[0.3,3.5]])
    labels = ['A','A','B','B']
    return group,labels

###通过KNN进行分类
def classify(input,dataSet,label,k):
    dataSize = dataSet.shape[0]
    ####计算欧式距离
    diff = tile(input,(dataSize,1)) - dataSet
    sqdiff = diff ** 2
    squareDist = sum(sqdiff,axis = 1)###行向量分别相加，从而得到新的一个行向量
    dist = squareDist ** 0.5
    
    ##对距离进行排序
    sortedDistIndex = argsort(dist)##argsort()根据元素的值从大到小对元素进行排序，返回下标

    classCount={}
    for i in range(k):
        voteLabel = label[sortedDistIndex[i]]
        ###对选取的K个样本所属的类别个数进行统计
        classCount[voteLabel] = classCount.get(voteLabel,0) + 1
    ###选取出现的类别次数最多的类别
    maxCount = 0
    for key,value in classCount.items():
        if value > maxCount:
            maxCount = value
            classes = key
    return classes

dataSet,labels = createDataSet()
dataSet1X=dataSet[0:2,0]
dataSet1Y=dataSet[0:2,1]
dataSet2X=dataSet[2:4,0]
dataSet2Y=dataSet[2:4,1]
plt.scatter(dataSet1X,dataSet1Y,color='blue')
plt.scatter(dataSet2X,dataSet2Y,color='red')
input = array([1.1,0.3])
plt.scatter(input[0],input[1],color='orange')
K = 3
output = classify(input,dataSet,labels,K)
print("测试数据为:",input,"分类结果为：",output)
plt.show()
```
代码实现的结果是，将以下橙色的点，通过KNN算法，分到了蓝色的点（K取3或者2）！即把点[ 1.1  0.3]分成为A类。 

![Alt text](http://pdcknxeeg.bkt.clouddn.com/201810172.jpg) 

在上面的例子，K的取值是（我）自己定的，但是做大规模机器学习时，K的取值是训练（试验）出来的，不可以自己随便定，要用数据来分析（如下面应用）。

##KNN的应用，使用KNN分类cifra-10数据集
第一次使用cifra-10的数据集（以后在写其他机器学习算法博客总结时，我会常用的），感觉像是手写体数字数据集的升级版，稍微比手写体数据集要高大上一点。  
简单介绍：  
CIFAR-10 是一个包含60000张图片的数据集。其中每张照片为32*32的彩色照片，每个像素点包括RGB三个数值，数值范围 0 ~ 255。  
所有照片分属10个不同的类别，分别是 'airplane', 'automobile', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck'  
其中五万张图片被划分为训练集，剩下的一万张图片属于测试集。  
下载链接：[http://www.cs.toronto.edu/~kriz/cifar.html](http://www.cs.toronto.edu/~kriz/cifar.html)
  
下面一步一步实现KNN分类cifra-10，为了简化程序，就取 K=1 的情况，当KNN的K值取1时，KNN就退化成为最近邻算法，意思是寻求与目标最近的训练样本，将目标划分到与之最近的训练样本所在的类别。 

首先构造K=1时的最近邻分类器，保存为NearestNeighbor.py文件：   
```
import numpy as np
 
class NearestNeighbor(object):
  def __init__(self):
    pass
 
  def train(self, X, y):
    """ X is N x D where each row is an example. Y is 1-dimension of size N """
    # the nearest neighbor classifier simply remembers all the training data
    self.Xtr = X
    self.ytr = y
 
  def predict(self, X):
    """ X is N x D where each row is an example we wish to predict label for """
    num_test = X.shape[0]
    # lets make sure that the output type matches the input type
    Ypred = np.zeros(num_test, dtype = self.ytr.dtype)
 
    # loop over all test rows
    for i in range(num_test):
      # find the nearest training image to the i'th test image
      # using the L1 distance (sum of absolute value differences)
      distances = np.sum(np.abs(self.Xtr - X[i,:]), axis = 1)
      min_index = np.argmin(distances) # get the index with smallest distance
      Ypred[i] = self.ytr[min_index] # predict the label of the nearest example
 
    return Ypred
```   

之后对cifra-10数据集进行预处理，和调用上一个NearestNeighbor.py进行分类：  

```
import numpy as np
from NearestNeighbor import NearestNeighbor
import pickle
def unpickle(file):
    fo = open(file, 'rb')
    dict = pickle.load(fo,encoding='latin1')
    fo.close()
    return dict
 
#get the training data
dataTrain = []
labelTrain = []
for i in range(1,6):
    dic = unpickle("C:\\Users\\FWF\\Desktop\\cifar-10-python\\cifar-10-batches-py\\data_batch_"+str(i))
    for item in dic["data"]:
        dataTrain.append(item)
    for item in dic["labels"]:
        labelTrain.append(item)
        
#get test data
dataTest = []
labelTest = []
#do not know why the path is not just right as above,add a extra\
dic = unpickle("C:\\Users\\FWF\\Desktop\\cifar-10-python\\cifar-10-batches-py\\test_batch")
for item in dic["data"]:
   dataTest.append(item)
for item in dic["labels"]:
   labelTest.append(item)
   
#print "dataTest:%d" %(len(dataTest))
#print "labelTest:%d" %(len(labelTest))
dataTr = np.asarray(dataTrain)
dataTs1_10 = np.asarray(dataTest[1:10])
labelTr = np.asarray(labelTrain)
labelTs = np.asarray(labelTest)
print('dataTr的size ： ',dataTr.shape)
 
nn = NearestNeighbor() # create a Nearest Neighbor classifier class
nn.train(dataTr, labelTr) # train the classifier on the training images and labels
Yte_predict = nn.predict(dataTs1_10) # predict labels on the test images
# and now print the classification accuracy, which is the average number
# of examples that are correctly predicted (i.e. label matches)
print('预测的分类 : ',Yte_predict)
print('实际的分类 : ',labelTs[1:10])
print('accuracy: %f' % ( np.mean(Yte_predict == labelTs[1:10]) ))

```

之前有提到过，因为KNN算法需要计算目标与每一个训练集的样本计算L1或者L2距离，所以当训练集过大时，计算量也很大，所以在这里，只计算测试集的前十张图片的类别，否则我的i3处理器电脑不知道要跑多久。。。最后的结果如下：  

 ![Alt text](http://pdcknxeeg.bkt.clouddn.com/201810173.png)  

可见，效果并不好！！！（计算量又大，效果（准确率）又不好（高））
以上是k=1的代码实现，如果要选择一个合适的K值，请参考链接[https://blog.csdn.net/a584688538/article/details/75949741](https://blog.csdn.net/a584688538/article/details/75949741)

##全文参考链接
###[https://www.cnblogs.com/daacheng/p/8082682.html](https://www.cnblogs.com/daacheng/p/8082682.html)
###[https://www.cnblogs.com/ybjourney/p/4702562.html](https://www.cnblogs.com/ybjourney/p/4702562.html)
###[https://www.cnblogs.com/irran/p/cifar-10.html](https://www.cnblogs.com/irran/p/cifar-10.html)
###[https://www.cnblogs.com/irran/p/cifar-10.html](https://www.cnblogs.com/irran/p/cifar-10.html)
###[https://blog.csdn.net/fanzonghao/article/details/81516088](https://blog.csdn.net/fanzonghao/article/details/81516088)
###[https://blog.csdn.net/Andrewseu/article/details/51099811?utm_source=blogxgwz0](https://blog.csdn.net/Andrewseu/article/details/51099811?utm_source=blogxgwz0)