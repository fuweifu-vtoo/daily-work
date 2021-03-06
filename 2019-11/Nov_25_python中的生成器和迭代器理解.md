Nov_25_python中的生成器和迭代器理解
====

1. python中生成器和迭代器的理解:
	 
	- 迭代器（iterator） 实现了__next__方法和__iter__方法的对象就是迭代器。
	
	- 生成器（generator），作用是，比如要创建一个列表，里面的元素是有规律的，那么不需要创建包含所有元素的一个列表，占用内存，我们可以创建一个生成器，用于边输出，边计算下面的的值，有点像函数但是不用return，而是用yield,只有__next__方法。  

2. 这也是pytorch中dataloader为什么用生成器来做的原因了,在dataset中打开图片的操作是在__getitem__()中。

3. [可迭代对象]()：但凡内置有__iter__方法的对象,都称为可迭代对象,可迭代的对象:str,list,tuple,dict,set,文件对象；

4. [迭代器对象]()：如list是一个可迭代对象，使用a = iter(list)之后，返回的是一个迭代器对象，此时可以使用next(a)获得迭代器对象中的值；

5. 如果取值次数超过源值的数量就会报错，报错的错误是：StopIteration。也可以自己模拟，raise StopIterator；

6. [生成器函数]()：外表看上去像是一个函数，但是没有用return语句一次性的返回整个结果对象列表，取而代之的是使用yield语句一次返回一个结果，生成器函数返回一个迭代器，本质为迭代器；

7. [生成器表达式]():类似于列表解析，但是方括号换成了圆括号，它们返回按需产生的一个结果对象，而不是构建一个结果列表。例如：print((x ** 2 for x in range(5)))，这返回的就是一个生成器表达式；

8. 在迭代器对象中，next(iter)和iter.__next__()的作用一样；在可迭代对象中，iter(iter)和iter.__iter__()的作用一样；

9. [生成器]()的本质为[迭代器]()，迭代器和生成器的优点就是节省空间，不用一次构造出整个结果列表。

10. sorted(list)返回的是一个list，而reversed(list)返回的是一个list的迭代器；

11. 生成器函数返回一个迭代器，生成器表达式是对内存空间的优化。他们不需要像方括号的列表解析一样，一次构造出整个结果列表。

12. 注意自己写[迭代器函数]()的时候，要知道__iter__()和__next__()函数内部应该怎样写；其实[生成器函数]()就是简化了[迭代器函数]()的写法，直接使用yield返回值，内部自己会转成__iter__()函数和__next__()函数；

13. [生成器函数中的yield]()，在每次调用时，会在yield处返回值，并保存内部状态，并挂起中断退出。在下一轮迭代调用时，从yield的地方继续执行；

14. set_iterator 和 list_iterator 在转换为可迭代对象时，都可以使用set()或者list();其他的也类似；

15. 使用迭代器完成斐波那契数列，需要实现一个类，类内含有__iter__()函数和__next__()函数，使用生成器完成斐波那契数列，只是要实现一个函数即可；所以可以认为两者本质一样，只是生成器会更方便；

15. 参考：[https://www.cnblogs.com/louyefeng/p/9430415.html]()
16. [https://blog.csdn.net/zhangshuaijun123/article/details/81901708]()
17. [https://blog.csdn.net/weixin_45086637/article/details/92799127]()
18. [https://www.zhihu.com/question/24807364]()