##python确定list中是否有指定的tuple

情景模拟：  
有一个 list 列表，list 中存储的是成对的 tuple ，如下所示：

```   
[('home',19),('blog',21),('todolist',6),('contact',12)]  

```  
现在要确定list中是否含有 tuple（home，XX）,经过思考，发现用 *in* 和 *not in* 关键字很难实现，因为不确定 XX 的数值具体是多少，最后发现可以用 **filter()** 来实现：

```
# filter 函数返回的也是一个 list
list_filter = VisitNumber.objects.filter(page_name='home')
#将 list 中唯一的一个 tuple 取出，用list_filter[0]
count_nums = list_filter[0]
# count_nums.count的值就是 XX 的值了，可以进一步对其操作
count_nums.count += 1
count_nums.save()
```

用上面的代码的条件是， tuple 本身都是一个类的实例对象，这样才会有字段属性 page_name 和 count ，此时才可以根据 page_name 来写过滤条件。  
如果不符合上面的情况，可以用下面的代码：

```
#首先定义一个list_test，假装不知道list_test中tuple的排列顺序。
list_test=[('home',19),('blog',21),('todolist',6),('contact',12)]
for i in range(len(list_test)):
    if 'home' in list_test[i]:
        print list_test[i].[1]
```
这个办法用的是for循环的办法，方法很笨但很有用，最后打印出 XX 的值。
最后我感觉用for循环还是有点太low了，不想总被某鑫说我写代码只会for循环和if，而且觉得filter函数能做的肯定不止这点，所以又想出来下面的办法：

```
def has_home(x):
       if 'home' in x:
           return x
def has_home2(x):
		return 'home' in x
list_test=[('home',19),('blog',21),('todolist',6),('contact',12)]
# filter()函数接受两个参数，过滤条件和需要过滤的项
list(filter(has_home,list_test))
#或者使用list(filter(has_home2,list_test))
```
总结，python中的filter函数还是很强大的！平时就要多想想，多想几种解决办法。

<center>
by [fuweifu](http://www.vtoo.pro)  
<center>
<center>
<img src="http://pdcknxeeg.bkt.clouddn.com/baiyuechu.jpeg" width="10%" height="10%" />
</center>


