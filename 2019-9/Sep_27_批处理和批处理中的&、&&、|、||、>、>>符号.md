##Sep_27_批处理中的&、&&、|、||、>、>>符号

----

&& 顺序执行多条命令，当碰到执行出错的命令后将不执行后面的命令

|| 顺序执行多条命令，当碰到执行正确的命令后将不执行后面的命令

&  顺序执行多条命令，而不管命令是否执行成功

|  管道命令 前一个命令的执行结果输出到后一个命令

\> 清除文件中原有的内容后再写入

\>> 追加内容到文件末尾，而不会清除原有的内容主要将本来显示在屏幕上的内容输出到指定文件中指定文件如果不存在，则自动生成该文件

转载自[批处理中的&、&&、|、||、>、>>符号](https://blog.csdn.net/thanklife/article/details/78849882)


以下为几个批处理的例子：
例子1：
```
@echo off      ###关闭回显
setlocal enabledelayedexpansion ###启动变量延迟，让批处理感知动态变化
for /r %%i in (*.*) do( ###for循环
set cc=%%~xi    ###%%~xi是文件扩展名，例如.pdf
echo %%~ni | findstr "JC" &&(  ###%%~ni是文件名或者文件夹名，顺序执行，在其中找到JC字符
set a=%%~ni               ##a为文件名
set "a=!a:JC=AV!"  ##变量a中的JC替换成AV
ren "%%i" "!a!.pdf"  ##重命名，将%%i命名为a.pdf
)
)
```
例子2：
```
@echo off&SetLocal EnableDelayedExpansion
For /f "delims=" %%i in ('dir /b') do (
	Set a=%%i
	Set b=!a:JC01=JC11! ###将a中的JC01字符串变成JC11字符串
	ren "!a!" "!b!" )
pause
```

注意的是：
  1）在for循环中的变量，要用!a!来表示(本来应该是%a%，因为开启了变量延迟，所以变成!a!)，在for循环之外用!a即可.
  2）在cmd中，是必须使用%i，在批处理中，是必须使用%%i。
  3）在启动了变量延迟的情况下，要将变量的%改为!号,所以是!a!不是%a%。
  4）批处理中的for循环：FOR [参数] %%变量名 IN (相关文件或命令)   DO 执行的命令，带参数时支持以下参数:/d /l /r /f，具体可以参考[cmd批处理的/d/l/r/f](https://blog.csdn.net/Jum_Summer/article/details/81449311)