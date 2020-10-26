## Python中的深拷贝，浅拷贝和赋值引用

1. 在python中，有深拷贝(from copy import deepcopy)，浅拷贝(form copy import copy)和赋值引用(a=b),在使用的时候需要注意；

2. 首先区分：[可变对象]()和[不可变对象]()，可变对象比如list()、set()，不可变对象比如int,float,str等；

3. 在下方讨论的对象都指的是：[可变对象]()，不可变对象的拷贝不存在深拷贝浅拷贝之分；

4. python中的赋值引用：创建一个对象，然后把它赋给另一个变量的时候，python并没有拷贝这个对象，而只是拷贝了这个对象的引用；
>     a=[1,2,3]
>     b=a
>     a.append(4)
>     print(b)    # b=[1,2,3,4]
>     # 对a改变，b也会同时改变

5. python使用copy.copy()浅拷贝，即import copy，拷贝了对象，但没有拷贝子对象，当原始数据alist中的可变子对象改变，c中的可变子对象也会发生改变；

>     import copy
>     alist = [1,2,3,[1,2,3],4]
>     c=copy.copy(alist)
>     alist.append(5)
>     print alist    # [1,2,3,[1,2,3],4,5]
>     print c        # [1,2,3,[1,2,3],4]
>     alist[3].append(6)
>     print alist    # [1,2,3,[1,2,3,6],4,5]
>     print c        # [1,2,3,[1,2,3,6],4]

6. python使用copy.deepcopy()深拷贝，包含对象里面的子对象的拷贝，原始对象的改变不会造成深拷贝里任何子对象的改变；

>     import copy
>     alist = [1,2,3,[1,2,3],4]
>     c=copy.deepcopy(alist)
>     alist.append(5)
>     print alist    # [1,2,3,[1,2,3],4,5]
>     print c        # [1,2,3,[1,2,3],4]
>     alist[3].append(6)
>     print alist    # [1,2,3,[1,2,3，6],4,5]
>     print c        # [1,2,3,[1,2,3],4]

7. 主要的概念：变量名、引用、对象(可变对象/不可变对象)、拷贝(深拷贝/浅拷贝)；

8. 浅拷贝还包括：a = L[:]（切片引用）和 调用对象的拷贝方法 a=L.copy()；

9. 深拷贝只有一种：a = copy.deepcopy(L), 浅拷贝有三种写法；

10. 总结：
	- 赋值引用没有拷贝对象，只是拷贝了对象的引用；
	- 浅拷贝拷贝了对象，但是没有拷贝对象内部的可变子对象；
	- 深拷贝拷贝了对象，也拷贝了对象内部的可变子对象；

11. 参考[https://www.jianshu.com/p/a8f1af357046](https://www.jianshu.com/p/a8f1af357046)