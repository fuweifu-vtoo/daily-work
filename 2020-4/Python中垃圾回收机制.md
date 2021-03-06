## Python垃圾回收机制

### Python垃圾回收

1. Python中万物皆对象，最形象的一张图：![](http://markdown.vtoo.pro/2019Dec1466232-20190527230033806-672176396.png)

2. 当Python的某个对象的引用计数降为0时,这个对象就会被回收；
>     a = [321,123]
>     del a

3. 解析 [del a]()：del a后，已经没有任何引用指向之前建立的[321,123]，该list对象引用计数变为0，用户不可能通过任何方式接触或者动用这个对象，当垃圾回收启动时，Python扫描到这个引用计数为0的对象，就将它所占据的内存清空；

4. 频繁的垃圾回收将大大降低Python的工作效率；

5. Python只会在特定条件下，自动启动垃圾回收（有一个垃圾回收启动阈值）；

6. del a 只会删除变量和引用，并不会删除对象，如果该对象还有其他引用，则同样可以被访问到；
>     a = [1,2,3]
>     b = a[-1]
>     del a[-1]
>     print(b)    # b = 3