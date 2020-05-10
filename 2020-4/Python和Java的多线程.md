## Python和Java的多线程

1. [进程]()是正在执行的计算机程序的[实例]()。每个进程都有自己的[内存空间]()，用来存储正在运行的指令，以及需要存储和访问才能执行的任何数据。

2. [线程]()是进程的组件，[可以并行运行]()。[一个进程中可以有多个线程]()，它们共享相同的内存空间，即父进程的内存空间。

3. 总结：进程大于线程，线程可以并行，并行的线程共享父进程的内存空间，多线程使用更为常用；

4. python多线程：经常被吐槽为伪多线程，python的多线程的效率没有java多线程高；python中的多线程其实并不是真正的多线程，如果想要充分地使用多核CPU的资源，在python中大部分情况需要使用多进程；

5. python多线程和java多线程的区别：单核情况下，一次只能有一个线程执行；多核情况下，在java中可以支持多个线程同时执行，但在Python中，一次只能一个线程；

6. GIL（Global Interpreter Lock）全局解释器锁：在非python环境中，单核情况下，同时只能有一个线程执行。多核时可以支持多个线程同时执行。但是在python中，无论有多少核，同时只能执行一个线程。究其原因，这就是由于GIL的存在导致的。

7. 某个线程想要执行，必须先拿到GIL，并且在一个python进程中，GIL只有一个。

8. python下想要充分利用多核CPU，就使用多进程。因为每个进程有各自独立的GIL，互不干扰，这样就可以真正意义上的并行执行（但是同一时刻中一个进程中只有一个线程在执行），在python中，多进程的执行效率优于多线程(仅仅针对多核CPU而言)；

9. python针对不同类型的代码执行效率也是不同的：
	- CPU密集型代码，python下的多线程对CPU密集型代码并不友好；
	- IO密集型代码，python中多线程能够有效提升IO密集型代码的效率；
	- 但现实中代码往往是IO密集+CPU密集；

10. Python处在多核cpu，且是CPU密集型代码时，可以使用python的多进程加速；

11. python中多线程使用：import threading；多进程使用：multiprocessing包；

12. 而对于java而言，java的多线程则可以充分利用到多核CPU；不像Python那样一个进程可以有多个线程，但一个进程中只能有一个GIL，即只能是一个线程在多核CPU中的某一个核中运行；

13. 多进程的优缺点：
	- 优点：可以并行的执行多个任务，提高运行效率，(可利用计算机多核)，空间独立，数据安全；
	- 缺点：进程的创建和销毁过程需要消耗较多的计算机资源，在需要频繁创建和删除进程的情况下，不宜使用多进程；

14. 参考：[python多线程详解](https://www.cnblogs.com/luyuze95/p/11289143.html)

15. 参考：[为什么老说python是伪多线程，怎么解决？](https://blog.csdn.net/melon0014/article/details/90372172?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2)