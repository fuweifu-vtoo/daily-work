# Crow框架安装报告

标签（空格分隔）： Crow

---
## 系统环境：
操作系统：macOS High Sierra 10.13.2
内存：16 GB 1867 MHz DDR3
##目标：
成功安装Crow框架
##安装步骤：
1.桌面上自建一个空文件夹Crow，在[https://github.com/ipkn/crow](https://github.com/ipkn/crow)上下载Crow框架的所有内容；打开终端，输入命令:

```
git clone https://github.com/ipkn/crow
```
2.进入下载的文件夹，cd crow/crow/，执行命令:
```
mkdir build
cd build
cmake ..
make
```
至此Crow框架初步安装完成

3.build文件夹里的examples文件夹里是已经编译完成的例子，可以直接跑下测试自己的Crow 框架是否已经初步安装成功：
```
cd build/examples/
./helloworld
```
成功运行的话就是服务器开始运行，打开浏览器，进入localhost:10080,可以看到上⾯面有`helloworld` 的提示语；

4.build⾥里里⾯面是已经编译好的⽂文件，如果⾃自⼰己想要编译的话，要下载boost库和下载gcc（版本要在4.8以上），首先下载安装boost库，终端上输⼊入命令：
```
brew install boost
```
5.然后是gcc高版本安装，（mac自带的gcc版本是4.2，需要更新）：
```
brew install gcc@6
```
安装完成后需要把路路径添加到系统中去，首先找到新装的版本的路径：
```
which gcc-6
```
然后进入etc文件夹，修改profile文件,把路径加入系统环境变量中：
```
sudo vi profile
```
在文件后加入 `export PATH="/usr/local/bin/g++-6:$PATH"`（这里加入的路径要根据自己的情况而来，我的是/usr/local/bin/g++-6），然后更新系统路径:
```
source profile
```
6.现在就可以编译Crow文件了，进入crow内的examples文件夹，执⾏下面的命令：
```
g++ example_chat.cpp -o example_chat -I../include -std=c++11 -lboost_system -lpthread
```
编译得到一个可执行文件，./example_chat执行文件就可以，然后在浏览器器上打开localhost:40080就可以看到send的交互界面，输入crow并send，然后在终端上可以接收到发送的信息。

7.官方测试办法测试是否安装成功，在终端的命令行进入build文件夹中，输入下面的命令进行测试：
```
ctest
```



**ps:我是在ubuntu上没有安装成功才转mac上，因为ubuntu在cmake时，-lpthread提示找不到，寻找了半天也没有找到解决办法，期间还试过系统学习cmake，企图在cmake时指定附带编译的库，最后也没有成功。**

<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>




