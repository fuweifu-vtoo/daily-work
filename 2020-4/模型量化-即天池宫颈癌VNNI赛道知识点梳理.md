## 模型量化-即天池宫颈癌VNNI赛道知识点梳理

1. 任务描述及难点：比赛要求对宫颈癌细胞学异常鳞状上皮细胞进行定位和分类，其中阳性、阴性细胞特征差异不明显，bbox尺寸变化很大。VNNI赛道要求使用Intel提供的量化工具对模型量化，同时考查推理速度和检测精度两个指标。

2. 解决方案：VNNI赛道采用EfficientDet D2网络，对训练好的模型权重进行量化，量化前删除影响量化的节点，并折叠BN层。量化权重由32bit到8bit，动态产生校准数据后再量化，减少量化带来的精度损失。在确保精度的前提下对图片Resize后再训练和预测，进一步加速推理速度。

3. 需要获得一个有效范围，再进行量化，不是直接从fp32道Int8：![](http://markdown.vtoo.pro/2019Decquantization1.jpg)![](http://markdown.vtoo.pro/2019Decquantization2.jpg)

4. 有些层不支持量化，VNNI会自动选择哪些进行量化哪些不进行量化，对于不量化的层，输入之前会有反量化；量化后的层，其输入输出都是Int8；![](http://markdown.vtoo.pro/2019Decquantization3.jpg)

5. 量化的模型和没有量化模型的预测代码是应该没有任何变化的；

6. 量化步骤：
### Step 1 找出图中所有可能的输入和输出节点名称
```
bazel-bin/tensorflow/tools/graph_transforms/summarize_graph \
     --in_graph=/workspace/quantization/test_code_ssd_resnet/frozen_inference_graph.pb \
     --print_structure=false >& model_nodes.txt
```
读取其中inputs， outputs节点名字；

### Step 2 冻结TF模型

将checkpoint文件转成graph(如果你的模型pb文件，已经冻结，跳过此步)

```
python tensorflow/python/tools/freeze_graph.py \
     --input_graph /workspace/quantization/frozen_inference_graph_ssd_resnet50.pb \
     --output_graph /workspace/quantization/freezed_graph.pb \
     --input_binary False \
     --input_checkpoint /workspace/quantization/<checkpoint_file> \
     --output_node_names OUTPUT_NODE_NAMES
OUTPUT_NODE_NAMES 就是上步得到的： 'detection_boxes,detection_scores,num_detections,detection_classes'
```

### Step 3 优化FP32模型

#### a.删除影响量化的node

bazel-bin/tensorflow/tools/graph_transforms/transform_graph \
     --in_graph=/workspace/quantization/frozen_inference_graphpb \
     --out_graph=/workspace/quantization/frozen_inference_graph_rm_id.pb  \
     --inputs='image_tensor' \
     --outputs='dtion_boxes,detection_scores,num_detections,detection_classes'  \
     --transforms='strip_ununodes remove_nodes(op=Identity, op=CheckNumerics)'
     
#### b.处理node share情况

```
cd /workspace/quantization/test_code_ssd_resnet
python shared_node.py frozen_inference_graph_rm_id.pb frozen_inference_graph_rm_shared.pb 
```

#### c.优化模型

```
cd /workspace/tensorflow
bazel-bin/tensorflow/tools/graph_transforms/transform_graph \
     --in_graph=/workspace/quantization/test_code_ssd_resnet/frozen_inference_graph_rm_shared.pb \
     --out_graph=/workspace/quantization/test_code_ssd_resnet/optimized_graph.pb \
    --inputs='image_tensor' \
    --outputs='detection_boxes,detection_scores,num_detections,detection_classes' \
    --transforms='strip_unused_nodes remove_nodes(op=Identity, op=CheckNumerics) fold_constants(ignore_errors=true) fold_batch_norms'
```
### Step 4 运行推理，获取模型精度信息
````
cd /workspace/quantization/test_code_ssd_resnet
python test.py optimized_graph.pb 
...
--- 690.8093204498291 seconds ---
mAP:  0.2389577100281054
```

## Int8量化的步骤
### Step 5: 量化优化过的模型
```
cd /workspace/tensorflow
python tensorflow/tools/quantization/quantize_graph.py \
     --input=/workspace/quantization/test_code_ssd_resnet/optimized_graph.pb \
     --output=/workspace/quantization/test_code_ssd_resnet/quantized_dynamic_range_graph.pb \
     --output_node_names='detection_boxes,detection_scores,num_detections,detection_classes' \
     --mode=eightbit \
     --intel_cpu_eightbitize=True \
     --excluded_ops=ConcatV2 
```
     
### Step 6: 使用从动态到静态再量化的过程，转换量化的图

#### a 生成带日志记录操作的模型
```
cd /workspace/tensorflow
bazel-bin/tensorflow/tools/graph_transforms/transform_graph \
 --in_graph=/workspace/quantization/test_code_ssd_resnet/quantized_dynamic_range_graph.pb \
 --out_graph=/workspace/quantization/test_code_ssd_resnet/logged_quantized_graph.pb \
 --transforms='insert_logging(op=RequantizationRange, show_name=true, message="__requant_min_max:")'
```

#### b 产生校准数据
```
cd /workspace/quantization/test_code_ssd_resnet
python test.py logged_quantized_graph.pb > min_max_log.txt 2>&1
```

#### c 使用校准数据代替RequantizationRangeOp
```
cd /workspace/tensorflow
bazel-bin/tensorflow/tools/graph_transforms/transform_graph \
--in_graph=/workspace/quantization/test_code_ssd_resnet/quantized_dynamic_range_graph.pb \
--out_graph=/workspace/quantization/test_code_ssd_resnet/freezed_range_graph.pb \
--transforms='freeze_requantization_ranges(min_max_log_file="/workspace/quantization/test_code_ssd_resnet/min_max_log.txt")'
```

#### d 运行推理，比较精度

### Step 7 优化量化的模型
根据上述模型精度的变化，针对量化过的模型（步骤6），重复步骤3， 找到合适的参数 --transforms option.

## 最后验证量化模型的性能
    使用最终量化的模型，运行推理，获取模型的精度。
    精度的目标是优化后的FP32模型的精度.
    量化后的精度损失应该在~0.5-1%以内.

7. 量化之后，比赛的其他要点是：是否要做窗口划窗？怎样做窗口滑窗？是否要对输入图片做Resize？如何权衡精度和性能的比例？以及如何进行多线程推理加速？



