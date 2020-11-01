## 准确率、精确率、召回率、F-score、MIoU、ROC、PR曲线和OA

1. 准确率(accuracy):
	- 所有的 （预测正确的） 占 （总的） 的比重。
	- 在样本不均衡时，高准确率没有任何意义，此时准确率会失效
	- ![](http://markdown.vtoo.pro/2019Dec0411_accuracy.png)
	- 这个准确率又叫 overall accuracy！不是相对于某一类而言的准确率，是相对所有类而言；

2. 精确率(precision):
	- 正确预测为正的占全部预测为正的比例
	- ![](http://markdown.vtoo.pro/2019Dec0411_pr.png)
	- 精确率(Precision) 针对预测结果而言的
	- 精确率使相对于某一个而言，可以求得average precision；

3. 召回率(recall)：
	- 正确预测为正的占全部实际正的比例
	- ![](http://markdown.vtoo.pro/2019Dec0411_recall.png)
	- 召回率(Recall) 针对原样本而言的
	- 精确率和召回率在实际情况是两者相互“制约”

4. F1-score:精确度和召回率之间是矛盾的，所以引入F1-Score作为综合指标，为了平衡准确率和召回率的影响
	- ![](http://markdown.vtoo.pro/2019Dec0411_f_score.png)
	- F1是精确率和召回率的调和平均。

5. F-score:![](http://markdown.vtoo.pro/2019DecFscore.png),其中β是用来平衡Precision,Recall在F-score计算中的权重；
	- 如果取1,表示Precision与Recall一样重要，即F1-score
	- 如果取小于1,表示Precision比Recall重要！
	- 如果取大于1,表示Recall比Precision重要，常见于医疗诊断中；

6. 语义分割中的MIoU：![](http://markdown.vtoo.pro/2019Decmiou.png)
	- 同样也考虑到了精确度和召回率两者
	- 公式和F1-score很像
	- 直观理解是：交集 部分除以 真实值和预测值的并集；

7. 无论是F-score 还是 IoU，都可以针对某一个类别求取，第一步一定是求混淆矩阵；之后可以所有的类做平均，求 avg-F-score 和 MIoU；

8. MIoU的计算：见9和10；

9. 步骤一 先求出混淆矩阵：![](http://markdown.vtoo.pro/2019Dec20190522193419885.png)

10. 步骤二 再求MIoU,使用混淆矩阵的每一行再加上每一列，最后减去对角线上的值：![](http://markdown.vtoo.pro/2019Dec微信图片_20200411160028.png)

11. 两个曲线：ROC曲线和PR曲线；
	- ROC曲线横轴为假正率（FP_rate）和纵轴假负率（TP_rate），曲线与FP_rate轴围成的面积（记作AUC）越大，说明性能越好
	- ROC曲线在高不平衡数据条件下的表现不如PR曲线；
	- PR曲线MAP中用的多；

12. ROC曲线：![](http://markdown.vtoo.pro/2019Dec20181009162820313.png)
	- AUC越大，性能越好，即图上L2曲线对应的性能优于曲线L1对应的性能
	- A点是最完美的performance点，B处是性能最差点

13. Accuracy,precision和Recall参考：[准确率、精确率、召回率和F-score](https://blog.csdn.net/Joseph__Lagrange/article/details/90813885)   

14. F-score参考：[分类的评判标准F-score](https://www.jianshu.com/p/f7ea71f2344f)

15. MIoU参考：[语义分割中的MIoU](https://blog.csdn.net/u012370185/article/details/94409933)

16. OA指的是（overall）accuracy，AA（average accuracy）指的是 average precision；