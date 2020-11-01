##July_30_ubuntu中多个cuda版本共存

1. momentum 动量系数，
2. power系数，用于更新学习率
3. 好像在conda的虚拟环境中，无论是用pip（只要确保这个pip是属于这个conda虚拟环境的python）还是conda，都只会在这个虚拟环境生效，不会影响到虚拟环境之外。pip安装的包在虚拟环境对应的lib文件夹中（如果是base的话，就在anaconda自身的lib）。
4. 查看cudnn版本
```c
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```
5. 查看cuda版本
```c
cat /usr/local/cuda/version.txt
```
或者直接**nvcc -V**
6. .bashrc中要添加环境变量：
```c
# <<< CUDA init <<<
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
#export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
```
7. 切换cuda10.1到cuda9.0
```c
sudo rm –rf /usr/local/cuda
sudo ln -s /usr/local/cuda-10.0 /usr/local/cuda
```
8. 用nvidia-smi看到的cuda的信息可能不准，因为切换回cuda9.0之后，上面的显示还是cuda10.2，而我只有cuda10.1.168。
9. 查看torch支持的cuda版本
```c
import torch
print(torch.version.cuda)
```
10. 查看torch的版本
```c
import torch
print(torch.__version__)
```
11. 现在的问题是，我的pytorch支持的cuda的版本是10.0.130（也就是cudatoolkit，当时安装pytorch是需要用cuda编译的），但是我的cuda版本是10.1.168，这也是我不能编译成功一些pytorch支持的第三方库的原因，例如‘apex’。而且现在也不是下载一个cuda10.0.130就能成功的事，因为pytorch也需要重新编译重新更换（也有可能是下载下来之后就编译好的）。但是如果不用编译第三方库，基本功能还是一切正常，cuda10的基本能通用。参考链接[https://github.com/NVIDIA/apex/pull/323](https://github.com/NVIDIA/apex/pull/323),我这也叫：Hard error on Pytorch Cuda + Cuda toolkit version mismatch。并不是cuda10就通用的。

12. 还可以用‘’nvidia-smi‘’查看驱动支持的cuda的版本，可以看到我的driver支持的驱动的版本是cuda10.2。所以总结一下：我anaconda的base的环境是：**Your pytorch is compiled for cuda 10.0, your driver is cuda 10.2, yet your compiler (with which you compile apex) is 10.1.**。参考链接：[https://github.com/NVIDIA/apex/issues/314](https://github.com/NVIDIA/apex/issues/314)。所以我现在最快的做法，应该是安装一个cuda10.0，之后能不能成功编译‘’apex‘’。

13. 可能是我驱动的版本太高了，我发现我不能使用：pytorch0.4.1（build for cuda9.0）+cuda9.0，但是我的cuda9.0是能够正常使用的，只是pytorch这个有问题，但是不知道问题出在哪里了。报错：cuda runtime error (11) : invalid argument at /opt/conda/conda-bld/pytorch_1535493744281/work/aten/src/THC/THCGeneral.cpp:663。参考链接[https://discuss.pytorch.org/t/cuda-runtime-error-11/30080](https://discuss.pytorch.org/t/cuda-runtime-error-11/30080)

14. 还有一个cuda toolkit的版本,好像要和cuda的版本一致,这个在安装框架的时候经常会需要安装cuda toolkit.(cuda toolkit的版本不知道怎么查看)

15. [显卡，显卡驱动,nvcc, cuda driver,cudatoolkit,cudnn区别](https://cloud.tencent.com/developer/article/1536738),里面还解释了nvcc和nvidia-smi显示的CUDA版本不同的问题.