##自动驾驶比赛的目标检测说明文档

### mmdetection配置  
- 按照https://github.com/open-mmlab/mmdetection/blob/master/docs/INSTALL.md配置好mmdetection
- 下载coco预训练模型,链接https://open-mmlab.s3.ap-northeast-2.amazonaws.com/mmdetection/models/hrnet/cascade_rcnn_hrnetv2p_w32_20e_20190522-55bec4ee.pth  
- 在mmdetection目录下创建models文件夹,将下载的coco预训练模型拷贝到models文件夹
- 把代码中的my_dataset.py拷贝到mmdetection/mmdet/datasets/目录下,
  并将该目录下的__init__.py替换成提供代码中的__init__.py

### 数据准备 
- 将下载的数据train_dataset.zip和DF_1018.tar.gz与test_dataset.zip放置在统一文件夹,
- 将代码中的transform_dataset.sh,create_train_val.py,merge_anno.py,
  create_test_info.py放置于统一文件夹.
- 运行transform_dataset.sh得到合并后的两次数据集图片文件夹train2017,
  验证集文件夹val2017(数据与训练集相同),与测试数据文件夹test2017,
  以及转换后的coco格式的标注文件夹annotations
- 在mmdetection目录下创建data/coco目录,将上一步得到的4个文件夹train2017,val2017,test2017,
  annotations拷贝到/mmdetection/data/coco/目录下


### 训练:
- 将代码中的ga_albu_cascade_rcnn_hrnetv2p_w32_20e_oh.py拷贝到mmde/configs/目录下,
  这个文件有我们所使用的网络的所有信息,已做注释
- 将代码中的dist_train.sh拷贝到mmdetection目录下,运行dist_train.sh脚本
  默认使用双卡(两张1080Ti)训练,训练之前会自动下载imagesnet预训练的模型,可能需要花一点时间
  
### 测试:
- 将代码中的test.sh与save_res_txt.py拷贝到mmdet目录下,运行test.sh得到测试集输出结果test_res.bbox.json
- 运行save_res_txt.py,得到txt格式的输出结果test_res.txt

