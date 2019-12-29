##Oct_17_python的pickle模块

1. 在某天看到`RESULT_FILE: Filename of the output results in pickle format`的话的时候,对pickle模块好像似曾相识,但是又不常用,还是决定学习一下并且记录下来加深印象,但是不是特别深入的学习,只是记住常见用法.

2. pickle模块可以将对象以文件的形式存放在磁盘上,python中几乎所有的数据类型（列表，字典，集合，类等）都可以用pickle来序列化.

3. 机器学习中，常常需要把训练好的模型存储起来，这样在进行决策时直接将模型读出，而不需要重新训练模型，这样就大大节约了时间。Python提供的pickle模块就很好地解决了这个问题，它可以 序列化对象 并保存到磁盘中，并在需要的时候读取出来，任何对象都可以执行序列化操作.

4. Pickle模块中最常用的函数为:
1)pickle.dump(obj, file, [,protocol]),将obj对象序列化存入已经打开的file中.
2)pickle.load(file),将file中的对象序列化读出.
3)pickle.dumps(obj[, protocol]),将obj对象序列化为string形式，而不是存入文件中.
4)pickle.loads(string),从string中读出序列化前的obj对象.

5. 上述函数参数讲解:
	- obj：想要序列化的obj对象
	- file:文件名称
	- protocol：序列化使用的协议。如果该项省略，则默认为0。如果为负值或HIGHEST_PROTOCOL，则使用最高的协议版本。
	- string：文件名称

6. dump() 与 load() 相比 dumps() 和 loads() 还有另一种能力：dump()函数能一个接着一个地将几个对象序列化存储到同一个文件中，随后调用load()来以同样的顺序反序列化读出这些对象.

7. 参考[python数据存储：pickle模块的使用讲解](https://www.cnblogs.com/fmgao-technology/p/9078918.html)

8. pickle的文件存储完之后,其可读性非常差,只能用pickle load之后才可读,直接打开可读性非常差.