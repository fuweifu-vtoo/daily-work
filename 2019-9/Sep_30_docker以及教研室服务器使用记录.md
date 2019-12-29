##Sep_30_docker以及教研室服务器使用记录

###账号密码
1. 服务器ip:121.48.162.252,端口:8888,管理员名:root，密码:sitonholy,登录指令ssh root@121.48.162.252
2. Harbor镜像仓库服务器:admin,密码:Harbor12345
3. 服务器个人账号:fwf,密码:fuweifu123

###docker镜像操作步骤
1. 下载镜像,封装镜像,上传镜像
	- docker pull 121.48.162.252:8888/cvnlppro/cvnlp:v1
	- docker images | grep segmentation (查看本地镜像)
	- docker run -i -t 121.48.162.252:8888/cvnlppro/cvnlp:v1 /bin/bash (使用镜像创建docker容器,每次都会更新容器id,总结就是:镜像运行一个容器,并在容器里运行/bin/bash)
	- exit (退出当前容器)
	- docker ps -al (显示最近创建的容器,可查看容器id)
	- docker commit -m="cvnlp" -a="fwf" 08c2346f8d33 cvnlp:v2 (将运行中的容器封装成新的镜像,这个操作不会关闭容器)
	- docker images | grep cvnlp (查看本地镜像)
	- docker tag cvnlp:v2 121.48.162.252:8888/cvnlppro/cvnlp:v2 (给镜像打新的tag,镜像id不会变,只是tag改变了;cvnlp:v1和cvnlp:v2的镜像id不同,但是cvnlp:v2和 121.48.162.252:8888/cvnlppro/cvnlp:v2的镜像id相同,只是tag不同)
	- docker push 121.48.162.252:8888/cvnlppro/cvnlp:v2 (镜像上传到Harbor服务器)

2. 一些说明:
	- 镜像id,用docker images查看
	- 正在运行的容器id用docker ps -al查看.
	- 删除镜像,用docker rmi 225d459edf2e(镜像id),只能删除没有被容器关联的镜像
	- 删除镜像只能删除没有多余tag的镜像,否则要先删除多余的tag,用docker rmi 121.48.162.252:8888/cvnlp:latest 来删除tag,提示Untagges.
	- docker ps -al为显示最近创建的容器,主要是为了查看容器id,docker ps为显示所有的容器,可查看所有容器id.docker ps -n 5,查看最近创建的5个容器,使用docker stop 4b094cd434b0 可以关闭该容器,但是没有删除,下次想重新使用就docker start 4b094cd434b0,重启后,用docker exec -ti 4b094cd434b0 /bin/bash在容器中重新执行/bin/bash.停止后使用docker rm 4b094cd434b0删除该容器.
	- 我们还可以从 Docker Hub 网站来搜索镜像，Docker Hub 网址为： https://hub.docker.com/

3. 警告:不要在服务器中做任何重启、关机动作。包括重启docker，效果和重启服务器一样，会造成正在运行的程序停止！！！
	- docker启动命令 : systemctl start docker
	- 守护进程启动 : sudo systemctl daemon-reload
	- 重启docker : systemctl restart docker
	- 重启docker服务 : sudo service docker restart
	- 关闭docker服务 : service docker stop
	- 关闭docker : systemctl stop docker

4. [docker菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)

5. 我最常用的镜像:121.48.162.252:8888/cvnlppro/cvnlp:v1,其中的配置为:ubuntu18.08,x86_64,pytorch_1.1.0,cuda_version_10.0.130,python_3.6.8,no_conda,pip_version_19.3.1.


###提交作业

1. 首先需要将镜像pull到本地,不同的用户可以使用同一个本地镜像，最后只是node会有所不同.
2. 使用表格方式提交作业,最后执行命令:ls && sleep infinity,内存和共享内存保持一样大.
3. 一般作业名:homework_1,作业中的任务名:task_1
4. 我认为一个作业中多个任务是在任务和任务之间需要做通信才需要的,平时一个作业中包含一个任务就好了.

###服务器连接方式
1. SSH:通过网页进入(不用配置私钥)或者使用ssh资讯下载私钥进入
2. Jupyter:
3. 专业版Pycharm:
4. 远程桌面:

###pycharm链接服务
1. 具体参考[537服务器使用教程pycharm版本 - 简书_files](https://www.jianshu.com/p/a7922cca0df5),防止网页被删除,也保存了html文件在本地.
2. 大概的步骤是:
	- 从ssh资讯处下载私钥,并记住ip地址和端口号
	- 从pycharm工具栏上方tools-->deployment-->configuration中,-->Connection处通过key pair(私钥)建立新建SFTP连接,-->mapping处配置本地工程和远程路径(root/data/project_name)
	- tools-->deployment-->browse remote host打开远程host目录
	- 左边project文件夹处右击,选择deployment-->upload to ...上传整个文件夹
	- 选择远程编译器,file-->setting-->project interpreter-->show all--> '+' -->ssh interpreter-->existing server configuration-->next-->sync folders左边修改成本地路径,右边修改成远程路径-->finish
3. 注意:远程的编译器也可以直接通过pycahrm安装库和包，也可以通过ssh连接之后用pip安装包,但是关闭作业之后就没了，镜像在这里永远不会被更改，安装的库也只是暂时的.






