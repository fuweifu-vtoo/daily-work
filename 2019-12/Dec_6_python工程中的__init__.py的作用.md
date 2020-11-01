python工程中的__init__.py的作用
====

1. __init__.pyyongl用来标识该目录是一个python的模块包（module package），如果没有__init__.py，则这个文件夹中的文件不可以被import．

2. 如果目录中包含了 __init__.py 时，当用 import 导入该目录时，会首先执行 __init__.py 里面的代码.

3. __init__.py可以用来控制模块导入，常见的做法是：平时import只import这个文件夹（module package），但是在文件夹内部的__init__.py可以写上具体要import的py文件，这样可以有选择的import，达到控制模块导入的功能．

4. 但是有一个注意事项，在执行import时，当前目录是不会变的（就算是执行子目录的文件），还是需要完整的包名．例如在__init__.py中应该写，from models.resnet import resnet34 , 而不是直接from resnet import resnet34

5. 就算是同级别的py问价，但是一旦包含它们的文件夹变成了module package,即同级存在__init__.py,则都要写成from models.resnet import resnet34．而不是from resnet import resnet34．还可以写成from .resnet import resnet34.

6. __all__ 关联了一个模块列表，当执行 from xx import * 时，就会导入列表中的模块。可以将 __init__.py 中的代码修改为　__all__ = ['resnet34', 'resnet101'],这样便不会import resnet18 了．也可以把__all__ = ['resnet34', 'resnet101']放在resnet.py中．

7. 可以将初始化代码放入__init__.py文件中.

8. 在一个工程中，当有文件夹变成了module package，推荐在＿＿init__.py中写入from . import backbone和from .model_zoo import get_model, get_model_list，前者是对文件夹而言，后者是对文件而言．

9. 另外，一个文件可以被import的东西为类\方法＼变量＼结构体，不要import的很散，注意结构化．一个代码的结构可以自己去设计．

10. 目前能想到的最佳的两个办法：
	- 当文件变成了module package，下一级遇到文件夹就from . import backbone，将文件夹也变成module package，并新建一个__init__.py文件．遇到文件就from .deeplabv3 import *，然后在deeplab.py文件中用__all__来控制要被导入的类\方法＼变量＼结构．如果py文件想要导入自己上一层的文件夹中的文件，便用sys.path.append上一级目录再导入.(要导入的东西根据py文件中的__all__来变化)
	- 当文件变成了module package，下一级遇到文件夹就from . import backbone，将文件夹也变成module package，并新建一个__init__.py文件．无论是遇到文件还是遇到文件夹，都会写具体自己要import什么东西，不用__all__，例如from .model_zoo import get_model, get_model_list.(要导入的东西根据__init__.py文件中的import来变化)

11. 推荐使用第二种，好懂．且有时候用了第一种，也会在py文件中多写多添加一些第一种的东西以便好懂一些．

12. 按照第一种方法，虽然能够一劳永逸，直接from utils import *，但是不建议这样，最好是需要import什么用什么，当需要import的东西太多了，再from utils import *．

13. 发现经过上面的操作之后，我的包就变得像网上发布过的包一样，可以from PIL import Image,也可以直接from PIL import *,不可以import Image.哈哈哈那就是说明真正的包也是这样发布这样写的．