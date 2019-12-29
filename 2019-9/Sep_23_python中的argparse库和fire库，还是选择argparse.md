Sep_23_python中的argparse库和fire库，还是选择argparse

1. argparse的标准用法：
```
import argparse
parser = argparse.ArgumentParser(description="PyTorch DeeplabV3Plus Training")
parser.add_argument('--backbone', type=str, default='resnet',
choices=['resnet', 'xception', 'drn', 'mobilenet'],
help='backbone name (default: resnet)')
args = parser.parse_args()
print(args.backbone)
```
调用的时候，使用 `python main.py --backbone drn`

2. argparse是一个{命令行参数解析}模块。默认可以使用`python main.py -h` 或者 `python main.py -help`

3. fire的基本使用：

```
import fire
def train(a,b):
	return(a + b)
def test(c):
	return 2*c
if __name__ == "__main__":
	fire.Fire()
```
调用的时候，使用`python main.py train 1 2` 或者 `python main.py train --a 1 --b 2`, 后面这种更常见，在大型项目中，也会有，比如：`python main.py train --plot-every=150 --batch-size=128 --pickle-path='tang.npz' --lr=1e-3 --env='poetry3' --epoch=50`

4. Python Fire是一个Python库，只需对Fire进行一次调用即可将任何Python组件转变为命令行界面。

5. fire可以选择性的将程序的全部暴露给命令行还是部分暴露给命令行。详情见[https://blog.csdn.net/qq_17550379/article/details/79943740](https://blog.csdn.net/qq_17550379/article/details/79943740)

6. fire.Fire()、fire.Fire(<fn>)、fire.Fire(<dict>)、fire.Fire(<object>)、fire.Fire(<class>)等