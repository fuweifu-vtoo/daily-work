##Sep_9_torchvision中下载的模型的默认保存路径

----

1. torchvision中下载的模型的默认保存路径为：/home/vtoo/.cache/torch/checkpoints/

2. 定义预训练模型时会把权值下载到一个缓存文件夹中，这个缓存文件可以通过环境变量TORCH_MODEL_ZOO来指定。更多细节见torch.utils.model_zoo.load_url()。

3. 有些模型在训练和测试阶段用到了不同的模块，例如批标准化（batch normalization）。使用model.train()或model.eval()可以切换到相应的模式。更多细节见train()或eval()。

4. 所有的预训练模型都要求输入图片以相同的方式进行标准化，即：小批（mini-batch）三通道RGB格式（3 x H x W），其中H和W不得小于224。图片加载时像素值的范围应在[0, 1]内，然后通过指定mean = [0.485, 0.456, 0.406]和std = [0.229, 0.224, 0.225]进行标准化。