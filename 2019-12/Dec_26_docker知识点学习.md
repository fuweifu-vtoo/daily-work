# docker知识点学习

1. 查看docker版本: docker --version/docker -v

2. [docker pull下来的镜像放哪儿了？](https://www.cnblogs.com/Rightsec/p/10222950.html)

3. 可以在自己电脑上安装docker,也可以在阿里云服务器上安装docker.

4. 开通阿里云容器镜像服务:[https://www.aliyun.com/product/acr?](https://www.aliyun.com/product/acr?)

5. 构建镜像,并推送,可以使用[ Alibaba Cloud Toolkit](),也可以自己用docker build.建议直接使用docker build就好.

6. 安装完成Docker后，默认已经启动了docker服务，如需手动控制docker服务的启停，可执行如下命令:
	- sudo service docker start # 启动
	- sudo service docker stop # 停止
	- sudo service docker restart # 重启

7. 安装完docker之后,docker pull 镜像,报错:Cannot connect to the Docker daemon. Is the docker daemon running on this host?
解决办法: 使用`sudo su`切换到root用户,才能使用docker pull.(目前也就是只有root有能力使用docker)

8. [docker-io, docker-ce, docker-ee 区别](https://blog.csdn.net/zsy_1991/article/details/90261419):
	- docker.io ismaintained by Ubuntu
	- docker-engine ismaintained by Docker
	- 我现在自己电脑和阿里云推荐的是安装docker.io

9. Ubuntu 系统安装docker:
```python
sudo apt-get update
sudo apt install docker.io
```

10. 自行构建镜像要确保安装curl.`sudo apt-get install curl`

11. docker images | grep segmentation (查看本地镜像)

12. 运行docker指令: docker run -i -t 121.48.162.252:8888/cvnlppro/cvnlp:v1 /bin/bash

## 阿里云容器镜像服务

1. 我其中一个仓库的网址:[https://cr.console.aliyun.com/repository/cn-shanghai/tianchi-competition/for_cervical/details](https://cr.console.aliyun.com/repository/cn-shanghai/tianchi-competition/for_cervical/details)

2. 如何免费申请服务参考:[【入门】Docker练习场,手把手超详细操作说明](https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12586973.0.0.1f232a3cpeutNb)

## 构建镜像并推送

1. 准备所需文件,放在一个文件夹中,有:
	- Dockerfile 配置文件
	- run.sh,里面写着执行代码的指令,eg: python inference.py
	- 要运行的py文件,例如inference.py
	- 其他运行程序需要的数据

2. 在Dockerfile要指定工作目录.

3. 构建镜像
	- docker build -t registry.cn-shenzhen.aliyuncs.com/test_for_tianchi/test_for_tianchi_submit:1.0 .
注意：registry.~~~是上面创建仓库的公网地址，用自己仓库地址替换.最后的.是构建镜像的路径，不可以省掉。

4. 验证镜像是否正常运行:
	- CPU镜像：docker run your_image sh run.sh
	- GPU镜像：nvidia-docker run your_image sh run.sh

5. 登录到阿里云镜像仓库:
	- sudo docker login --username=17707043046 registry.cn-shanghai.aliyuncs.com

6. 推送镜像到阿里云镜像仓库:
	- docker push your_image

7. 参考[【入门】Docker练习场,手把手超详细操作说明](https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12586973.0.0.1f232a3cpeutNb)