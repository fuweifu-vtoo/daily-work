Sep_21_python中的\*和\**的作用

1. Python中，（\*）会把接收到的参数形成一个元组，而（\**）则会把接收到的参数存入一个字典。

	我们可以看到，foo方法可以接收任意长度的参数，并把它们存入一个元组中

```
\>>> def foo(*args):
...     print(args)
...
\>>> foo("fruit", "animal", "human")
('fruit', 'animal', 'human')
\>>> foo(1, 2, 3, 4, 5)
(1, 2, 3, 4, 5)
\>>> arr = [1, 2, 3, 4, 5]  # 如果我们希望将一个数组形成元组，需要在传入参数的前面 加上一个*
\>>> foo(arr)
([1, 2, 3, 4, 5],)
\>>> foo(*arr)
(1, 2, 3, 4, 5)
```

2. （**）将接收到的参数存入一个字典

```
>>> def foo(**kwargs):
...     for key, value in kwargs.items():
...         print("%s=%s" % (key, value))
...
>>> foo(a=1, b=2, c=3)
a=1
b=2
c=3
```

3. （\*）和（\**）一起使用

```
>>> def foo(*args, **kwargs):
...     print("args:", args)
...     print("kwargs:", kwargs)
...
>>> foo(1, 2, 3, a=1, b=2)
args: (1, 2, 3)
kwargs: {'a': 1, 'b': 2}
>>> arr = [1, 2, 3]
>>> foo(*arr, a=1, b=2)
args: (1, 2, 3)
kwargs: {'a': 1, 'b': 2}
```
name作为foo第一个参数，在调用foo("hello", 1, 2, 3, middle="world", a=1, b=2, c=3)方法时，自然而然捕获了hello，而middle必须经过指定关键字参数才可以捕获值，否则会和之前的参数一样形成一个元组

```
>>> def foo(name, *args, middle=None, **kwargs):
...     print("name:", name)
...     print("args:", args)
...     print("middle:", middle)
...     print("kwargs:", kwargs)
...
>>> foo("hello", 1, 2, 3, a=1, b=2, c=3)
name: hello
args: (1, 2, 3)
middle: None
kwargs: {'a': 1, 'b': 2, 'c': 3}
>>> foo("hello", 1, 2, 3, middle="world", a=1, b=2, c=3)
name: hello
args: (1, 2, 3)
middle: world
kwargs: {'a': 1, 'b': 2, 'c': 3}
>>> my_foo = {"name": "hello", "middle": "world", "a": "1", "b": "2", "c": "3"}
>>> foo(**my_foo)
name: hello
args: ()
middle: world
kwargs: {'a': '1', 'b': '2', 'c': '3'}
```

此外，我们还可以定义一个字典my_foo，并以foo(**my_foo)这样的方式，让name和middle各自捕获自己的值，没有捕获的则存入一个字典

当我们删除my_foo中的name，再像之前传入函数，函数会报错说需要name这个参数

```
>>> del my_foo["name"]
>>> foo(**my_foo)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: foo() missing 1 required positional argument: 'name'
```

参考博客[https://www.cnblogs.com/beiluowuzheng/p/8461518.html](https://www.cnblogs.com/beiluowuzheng/p/8461518.html)

4. 还可以用**把接收到的字典反存为a=b的参数形式作为参数传入：

```
>>>def fo(**arg):
...	print(arg)
>>>def foo(a,b):
...	print(a,b)
>>>my_dict = {'a':1,'b':2}
>>>fo(**my_dict)
>>>foo(**my_dict)

{'a': 1, 'b': 2}
1 2
```
这种形式的调用是深度学习中最常用的调用方式，要特别注意。


