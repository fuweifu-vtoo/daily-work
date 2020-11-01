# July 4
---------
##some about ubuntu command
- ubuntu中的less命令可以查看文件的内容，less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件。eg：less main.cpp
- mv同时移动多个文件至一个目录,可以用-t来指定目标文件夹。-t之前的都是需要移动的文件（夹），例如：mv  a.dir  b.dir   c.dir  index.html  zz.txt  -t  dest.dir,并且要注意mv命令是可以移动文件夹的。
---------
##some about CMAKE
- Cmake 的工具链非常简单:cmake+make，如果工程存在多个目录,需要确保每个要管理的目录都存在一个CMakeLists.txt。
- cmake运行make clean即可对构建结果进行清理，返回到make命令之前，在cmake命令之后，也就是build目录中会有四个文件，CMakeCache.txt  CMakeFiles  cmake_install.cmake  Makefile。
- cmake中的内部构建和外部构建的区别，但是要注意，通过外部编译进行工程构建,< projectname>_SOURCE_DIR 仍然指代工程路径,而 < projectname>_BINARY_DIR 则指代编译路径。在内部构建中，两者是相同的。这两个都是cmake中的变量，在定义工程名字的时候就会定义。
- 介绍4个简单的指令:PROJECT/MESSAGE/ADD_EXECUTABLE/SET 以及变量调用的方法,同时提及了两个隐式变量< projectname>_SOURCE_DIR 及< projectname>_BINARY_DIR,演示了变量调用的方法。
- 《cmake实践》这本书很好，目前看到了13页。
---------
##something about vim
- vim中，i、a、o都可以进入输入模式，++R进入取代模式++，：是进入底线命令模式，hjkl是上下左右，如果想要进行多次移动的话，例如向下移动 30 行，可以使用 "30j" 或 "30↓" 的组合按键。
- vim中换页用 ==ctrl+f== 和 ==ctrl+b==， n < space>表示光标会向后面移动 n 个字符距离。n< Enter>，光标向下移动 n 行。
- G是移动到档案中的最后一行，nG为移动到第n行，gg相当于1G，是移动到第1行。
- 0是到这一行的第一个，==shift+4==，也就是$，是到这一行的最后一个。
- :1,$s/word1/word2/g，表示从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2，其中s和g是固定的。
- ：w是保存，：q是退出，而：wq是保存加退出。
---------
##some about ubuntu
- which可以对可执行文件输出路径，例如 which pip，which python,which nvcc,which conda等