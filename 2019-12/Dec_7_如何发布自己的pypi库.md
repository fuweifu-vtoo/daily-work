##如何发布自己的pypi库

###账号密码配置
1. 注册一个pypi账号，我的账号为[fuweifu],密码[Fuweifu_123_],网址为：[https://pypi.org/](https://pypi.org/)

2. ps:这里密码是错的，大家不用试]，注册完账号之后，你还要进行邮箱验证．

3. 安装上传包要用的工具twine：pip install twine

###代码配置

4. 配置好自己的代码路径：
````bash
├── msctools
│   ├── __init__.py
│   └── tool.py
├── setup.py
├── README.MD
├── LICENSE
````
上面例子显示文件夹可以和py文件不同名.setup.py是setuptools的构建脚本。它告诉setuptools自己的包（例如名称和版本）以及要包含的代码文件。README.MD用来写包的描述,会在setup.py中被打开.__init__.py不可以缺少,里面要有from .tool import *;

5. 运行python setup.py check检查setup.py是否有错误，如果没报错误，则进行下一步．setup.py　的写法可以参考网上．

5. 编译打包自己的库，依次运行：python setup.py build, python setup.py sdist, python setup.py install．
	- python setup.py build: 编译(不用上传时,可以省略,直接运行第三步安装)
	- python setup.py sdist: setup.py同级目录生成一个dist文件夹,里面是压缩包,用于上传,除非是要上传,否则这步省略
	- python setup.py install: 安装到本地(若未执行第一,第二步,会先执行第一(第二)步)

6. 运行完python setup.py install，就已经在本地安装成功了，可以在本地进行测试代码是否正确．

7. 源码安装成功指的是,已安装到本地,可以在任意地方调用,删除原来的编译build文件夹和egg.info文件夹之后依然可以使用,此时和用pip安装的效果一样,安装在/home/vtoo/anaconda3/lib/python3.7/site-packages/中,卸载也只能用pip卸载.

7. 上传至pypi：twine upload dist/*，上传的时候会要求填写账号名和密码．

8. 上传完之后，会在自己的pypi账号中看到自己上传的project，再等待几分钟便能直接用pip下载到自己的包了．

###卸载package

1. 无论是用pip安装的还是用python setup.py install安装的,都是安装到python的第三方库中，可以直接import到，卸载都可以使用:pip uninstall lib.

参考[PyPI打包实践教程-2019](https://blog.csdn.net/zzzzlei123123123/article/details/100848703)
参考[如何发布自己的pypi库](https://blog.csdn.net/ice_moyan/article/details/89147850)
参考[https://www.cnblogs.com/numbbbbb/p/3584539.html](https://www.cnblogs.com/numbbbbb/p/3584539.html)