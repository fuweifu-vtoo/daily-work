## Sep_20_python中的lambda表达式和闭包概念

-----

1. lambda表达式，通常是在需要一个函数，但是又不想费神去命名一个函数的场合下使用，也就是指匿名函数。
	lambda所表示的匿名函数的内容应该是很简单的，如果复杂的话，干脆就重新定义一个函数了，使用lambda就有点过于执拗了。
	lambda就是用来定义一个匿名函数的，如果还要给他绑定一个名字的话，就会显得有点画蛇添足，通常是直接使用lambda函数。如下所示：

```
add = lambda x, y : x+y
add(1,2)  # 结果为3
```

2. 那么到底要如何使用lambda表达式呢？
	- 应用在函数式编程中
	Python提供了很多函数式编程的特性，如：map、reduce、filter、sorted等这些函数都支持函数作为参数，lambda函数就可以应用在函数式编程中。如下：
	需求：将列表中的元素按照绝对值大小进行升序排列,list1 = [3,5,-4,-1,0,-2,-6],sorted(list1, key=lambda x: abs(x))
	当然，也可以如下（只不过这种方式的代码看起来不够Pythonic）：
```
list1 = [3,5,-4,-1,0,-2,-6]
def get_abs(x):
    return abs(x)
sorted(list1,key=get_abs)
```

	- 应用在闭包中
```
def get_y(a,b):
     return lambda x:ax+b
y1 = get_y(1,1)
y1(1) # 结果为2
```
	当然，也可以用常规函数实现闭包，如下：
```
def get_y(a,b):
    def func(x):
        return ax+b
    return func
y1 = get_y(1,1)
y1(1) # 结果为2
```
	只不过这种方式显得有点啰嗦。
	那么是不是任何情况下lambda函数都要比常规函数更清晰明了呢？
	肯定不是。
	Python之禅中有这么一句话：Explicit is better than implicit（明了胜于晦涩），就是说那种方式更清晰就用哪一种方式，不要盲目的都使用lambda表达式。

转载自：[https://www.cnblogs.com/hf8051/p/8085424.html](https://www.cnblogs.com/hf8051/p/8085424.html)
