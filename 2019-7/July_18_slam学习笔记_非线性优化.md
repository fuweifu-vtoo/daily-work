##slam学习笔记_非线性优化

1. **现在已经知道了经典 SLAM 模型的运动方程和观测方程。现在我们已经知道,方程中的位姿可以由变换矩阵来描述,然后用李代数进行优化。观测方程由相机成像模型给出,其中内参是随相机固定的,而外参则是相机的位姿。**

2. **然而,由于噪声的存在,运动方程和观测方程的等式必定不是精确成立的,所以，我们需要讨论如何在有噪声的数据中进行准确的状态估计。**
3. ubuntu 安装 ceres库：
~~~c
sudo gedit /etc/apt/sources.list
deb http://cz.archive.ubuntu.com/ubuntu trusty main universe
sudo apt-get update
sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3.1.2 libgflags-dev libgoogle-glog-dev libgtest-dev
git clone https://ceres-solver.googlesource.com/ceres-solver
mkdir include
cd include
cmake ..
make
sudo make install
~~~
以上参考[Ubuntu16.04中安装ceres](https://blog.csdn.net/pancheng1/article/details/81104724)

4. 安装完成后，Ceres库的头文件安装在"/usr/local/include/ceres/"目录下,库文件安装在"/usr/local/lib/"目录下。

5. 对应g20库的安装，也需要安装第三方依赖，这些依赖也需要在/etc/apt/sources.list添加之前添加过的地址，添加过就不用重复了。总之步骤差不多，也是自己安装第三方依赖后编译安装：
~~~c
sudo gedit /etc/apt/sources.list
deb http://cz.archive.ubuntu.com/ubuntu trusty main universe
sudo apt-get update
sudo apt-get install libqt4-dev qt4-qmake libqglviewer-dev libsuitesparse-dev libcxsparse3.1.2
libcholmod-dev
git clone https://github.com/RainerKuemmerle/g2o
mkdir include
cd include
cmake ..
make
sudo make install
~~~

6. 安装完成后,g2o 的头文件将在/usr/local/g2o 下,库文件在/usr/local/lib/下。
7. g2o会安装在usr\local\bin,usr\local\include,usr\local\lib三个文件夹中,和Ceres是一样的。但是发现从github上下载的g2o进行编译之后，跑高翔的代码跑不通。可能是版本问题，现在需要做的是，卸载现在安装的g2o，安装一个新的g2o。
7. **方法一：**g2o会安装在usr\local\bin,usr\local\include,usr\local\lib三个文件夹中，进入这三个文件夹，在终端输入
~~~c
sudo rm -rf *g2o*
~~~

8. **方法二：**:① 删除/usr/local/include/g2o，指令为sudo rm -rf /usr/local/include/g2o；② 删除/usr/local/lib下有关libg2o_*.so的库文件，先进入目录cd /usr/local/lib，然后挨个（可多个同时）删除sudo rm -rf libg2o_*.so libg2o_*.so libg2o_*.so
9. **方法三：**在终端输入：
~~~c
sudo rm -r /usr/local/lib/libg2o* /usr/local/include/g2o /usr/local/lib/g2o /usr/local/bin/g2o*
~~~

10. **方法四：**研究了一下，找到一种比较高效的办法：找到之前的build文件夹，里面会有一个叫install_manifest.txt的纯文本文件，里面的内容就是你cmake安装的文件，我们只要以这个内容作为变量交给rm指令处理就好：
~~~c
cat install_manifest.txt | sudo xargs rm
~~~

11. 删除完这个版本的g2o之后，就安装编译高翔的这个版本g2o。以上问题的发现，参考于[跑高翔十四讲中涉及到g2o时的版本升级问题](https://blog.csdn.net/cugui5044/article/details/84554486)。
12. 本节介绍了 SLAM 中经常碰到的一种非线性优化问题:由许多个误差项平方和组成的最小二乘问题。介绍了它的定义和求解,并且讨论了两种主要的梯度下降方式: Gauss-Newton 和 Levenberg-Marquardt。在实践部分中,分别使用了 Ceres 和 g2o 两种优化库求解同一个曲线拟合问题,发现它们给出了相似的结果。