##使用python将图片白色背景变透明
将图片的白色背景变成透明色在做 ppt 的时候经常要用到，最常用方法是使用 ps ，这也是百度得到的最多的解决办法，如下图所示：
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/0821.png" width="50%" height="50%" />
</center>
但是作为一个 python 程序员，怎么能容能 PhotoShop 软件打开时的那十秒钟，况且，当需要批量处理图片时，用 photoshop 可能就有点 hold 不住，自然这时想到了使用程序来快速将图片的白色背景变透明。  

这里以 jpg 图像为例，但下面的程序用在其他格式的也是可以的。jpg 图像只有三个通道，即 RGB ，而目标图像的格式选择了带有 Alpha 通道的 png 图像，即 png 有 RGBA 四个通道。我们要做的就是给 jpg 图像转换格式，并添上 aplha 通道。  
下面贴上代码：

```
from PIL import Image
import os

class ImageProcess():
	def __init__(self,path_image,output_path):
	    self.path_image = path_image
	    self.output_path = output_path
	def rid_white_background(self):

		for filename in os.listdir(self.path_image):
		  if filename.endswith('.jpg'):	
		    img = Image.open(self.path_image+filename)
		    img = img.convert("RGBA")
		    print(self.path_image+filename)
		    pixdata = img.load()

		    for y in range(img.size[1]):
		        for x in range(img.size[0]):
		            if pixdata[x,y][0]>220 and pixdata[x,y][1]>220 and pixdata[x,y][2]>220 and pixdata[x,y][3]>220:
		                pixdata[x, y] = (255, 255, 255, 0)
		    #img.show()
		    img.save(self.output_path+filename.replace('.jpg','_')+"out.png", "PNG")

if __name__ == '__main__':
	path_image = "/Users/apple/Desktop/"
	output_path = "/Users/apple/Desktop/"
	imageprocess1 = ImageProcess(path_image,output_path)
	imageprocess1.rid_white_background()
	print("end")
```
使用时，只需要修改`path_image`、`output_path`和目标处理图片的格式（上例中是jpg格式），即可批量对图片的白色背景操作。

<center>
by [fuweifu](http://www.vtoo.pro)  
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>