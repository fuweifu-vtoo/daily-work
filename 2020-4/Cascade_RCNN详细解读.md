### Cascade_RCNN详细解读
-------------
## 一句话总结
Cascade R-CNN就是使用不同的IOU阈值，训练了多个级联的检测器；

1. cascade rcnn 结构图：
![](http://markdown.vtoo.pro/2019Deccascade_rcnn.jpg)
    
## 正负样本定义
1. 在基于anchor的检测方法中，我们一般会设置训练的正负样本（用于训练分类以及对正样本进行坐标回归），选取正负样本的方式主要利用候选框与ground truth的IOU占比，常用的比例是50%，即IOU>0.5的作为正样本，IOU<0.3作为负样本等；

2. 但是这样就带来了一个问题，阈值取0.5是最好的吗？作者通过实验发现：
	- 设置不同阈值，阈值越高，其网络对准确度较高的候选框的作用效果越好。
	- 不论阈值设置多少，训练后的网络对输入的proposal都有一定的优化作用。

3. 基于这两点，作者设计了Cascade R-CNN网络，即通过级联的R-CNN网络，每个级联的R-CNN设置不同的IOU阈值，这样每个网络输出的准确度提升一点，用作下一个更高精度的网络的输入，逐步将网络输出的准确度进一步提高。

## Faster RCNN 存在的问题   
1. 训练阶段和预测阶段的proposal分布不同(mismatch问题)：
	- training阶段，RPN网络提出了2000左右的proposals，这些proposals被送入到Fast R-CNN结构中，在Fast R-CNN结构中，首先计算每个proposal和gt之间的iou，通过人为的设定一个IoU阈值（通常为0.5），把这些Proposals分为正样本（前景）和负样本（背景），并对这些正负样本采样，使得他们之间的比例尽量满足（1:3，二者总数量通常为128），之后这些proposals（128个）被送入到Roi Pooling，最后进行类别分类和box回归。
	- inference阶段，RPN网络提出了300左右的proposals，这些proposals被送入到Fast R-CNN结构中，和training阶段不同的是，inference阶段没有办法对这些proposals采样（inference阶段肯定不知道gt的，也就没法计算iou），所以他们直接进入Roi Pooling，之后进行类别分类和box回归。

2. 解释mismatch问题: 
	- 在training阶段，由于我们知道gt，所以可以很自然的把与gt的iou大于threshold（0.5）的Proposals作为正样本，这些正样本参与之后的bbox回归学习。
	- 在inference阶段，由于我们不知道gt，所以只能把所有的proposal都当做正样本，让后面的bbox回归器回归坐标。

3. 可以明显的看到training阶段和inference阶段，bbox回归器的输入分布是不一样的，training阶段的输入proposals质量更高(被采样过，IoU>threshold)，inference阶段的输入proposals质量相对较差（没有被采样过，可能包括很多IoU<threshold的），这就是论文中提到mismatch问题，这个问题是固有存在的，通常threshold取0.5时，mismatch问题还不会很严重。

4. 虽然threshold取0.5时，mismatch问题还不会很严重，但是文章同样发现，更高的threshold对准确度较高的候选框的作用效果越好，所以提升threshold很有必要；Cascade_RCNN就是即提升了threshold，又解决了mismatch问题；

## 总结

1. 总结：RPN提出的proposals大部分质量不高，导致没办法直接使用高阈值的detector，Cascade R-CNN使用cascade回归作为一种重采样的机制，逐stage提高proposal的IoU值，从而使得前一个stage重新采样过的proposals能够适应下一个有更高阈值的stage。
	- 每一个stage的detector都不会过拟合，都有足够满足阈值条件的样本。
	- 更深层的detector也就可以优化更大阈值的proposals。
	- 每个stage的H不相同，意味着可以适应多级的分布。
	- 在inference时，虽然最开始RPN提出的proposals质量依然不高，但在每经过一个stage后质量都会提高，从而和有更高IoU阈值的detector之间不会有很严重的mismatch。

9. 全文参考：[Cascade R-CNN 详细解读](https://zhuanlan.zhihu.com/p/42553957)
