---
title: Batch基础教程
date: 2017-11-26 19:30:53
tags: [批处理]
categories: [教程]
---
本文介绍了Batch的基本知识，基本命令。通过这些知识构建NOIP竞赛中能够使用的验证程序。正式比赛中编写该程序以降低在现场赛中发生低级错误的可能性。也可以构建自动化的对拍程序验证程序的正确性。

本文的部分程序打包附在文末，需要的同学请自行下载。

<!-- more -->
# 基本知识
## 什么是Batch
Batch文件又称为批处理文件，以`.bat`为后缀，是一种能够被DOS系统中的命令行(cmd.exe)运行的脚本文件。其作用和地位类似于Unix系统中的Shell脚本。学会使用批处理/Shell能够更好地使用操作系统交互，实现许多用户界面程序不会提供的功能。除了基本的命令之外，Batch还支持`IF,FOR,GOTO`等分支操作，不过本文中不会涉及，有兴趣的同学可以自行学习。

我们可以在命令行之中直接编写Batch命令，回车去运行，也可以写入`.bat`文件中之后再运行。

在NOIP竞赛中一般不会涉及Batch编程，但是利用批处理的技术，我们可以很好地检查文件命名是否正确，是否通过样例，更进一步的，我们也可以使用Batch进行自动对拍。


## 新建Batch
在文件夹选项中选择不隐藏扩展名之后，直接新建文本重命名为`.bat`文件即可。可以编写好之后再重命名，也可以右键文件使用记事本或者其他编辑器进行编辑，比较推荐使用Notepad++。

Batch和其他代码一样都是文本文件。


## 打开命令行
第一种打开命令行的方式是`Win+R`输入`cmd`或者`powershell`，当不需要使用具体文件夹的文件时使用这种方式比较好。

第二种打开命令行的方式可以直接在具体文件中打开。在文件夹空白处`Shift+右键`，打开菜单，Win10中单击**在此处打开Powershell窗口**或者Win7/8中单击**在此处打开命令行**打开命令行窗口。


## 运行Batch
最简单的是双击直接运行`.bat`文件，也可以在命令行中利用命令直接运行。

假如文件夹下有程序`run.bat`那运行以下命令均可以运行该程序：
``` bat
run
run.bat
```

如果将`run.bat`所在的路径加入到环境变量**Path**中则可以在任意路径下直接运行该程序。


## Path
**Path**是Windows系统的环境变量，在命令行中若当前路径下没有找到需要的程序，Windows系统就会在**Path**中进一步进行寻找。常用的程序或者自己写出的工具可以加入**Path**中以随时随地方便使用。向**Path**新增路径的步骤为：

1. 打开我的电脑->右键我的电脑->属性
2. 高级系统设置->环境变量
3. 修改用户环境变量中的**Path**，Win10中新建输入即可，Win7/8中加入`;`然后加上新路径

## 练习
- 尝试使用两种方式打开命令行，尝试对某个文件夹`Shift+右键`打开命令行，进行观察
- 新建一个`.bat`程序，输入`echo "Hello, World!"`，尝试运行
- 尝试将本机中的g++编译器，将它所在的文件夹加入到**Path**中，运行命令行输入`g++ -v`检测

# Batch基本命令
## 帮助命令
帮助命令是十分重要的命令，学会帮助命令之后可以在命令行中自行查看工具的使用方法。以下命令为帮助命令的基本使用：
``` bat
help
cd /?
help cd
```

第一行的`help`命令会显示所有的基本命令和解释，后两行命令会显示`cd`命令的使用方法，`help`中显示的命令均可以利用后两行的命令显示使用方法，许多命令行程序也会提供基本的帮助。

## echo命令
**echo**就是回声的意思，后面跟上一个字符串时候就会直接输出后面这个字符串。

运行`.bat`程序的时候，程序会在命令行中再输出一遍所有执行的命令。如果在`.bat`中输入`@echo off`，这些命令就不会再输出了。

``` bat
@echo off
echo sss
help
```

后两句的命令本身不会被输出到命令行中，但是它们执行后的结果仍然会被输出到命令行中。

## fc命令
**fc**就是file compare，能够比较两个文件的差异，在Unix下对应的程序为**diff**，fc能够传入两个参数，即两个需要比较的文件的路径。

``` bat fc_sample.bat
@echo off
fc a.txt b.txt
if errorlevel 1 (
    echo different
) else (
    echo same
)
pause
```

上面这段代码能够比较`a.txt`和`b.txt`两个文件，如果两个文件不同就输出`different`否则输出`same`，其中`errorlevel`为0是相同，而上面的`if errorlevel 1`是看`errorlevel`是否大于等于1。**注意`else`不可以再换行**。

## 变量
Batch中变量赋值通过`set`进行，默认为字符串，可以通过`/a`指定是整数，变量通过在两边加`%`来使用。
``` bat var_sample.bat
@echo off
set a=111
echo %a%.cpp
set /a b = 5
set /a b = %b% + 1
echo %b%
pause

=== output:
111.cpp
6
```
上面这段代码赋值了变量`a`然后接上`.cpp`后输出，然后赋值`b`为整数5，加一后输出。可以看出Batch当中字符串直接写在一起就是连接了。**注意字符串赋值的时候等号两边不能加空格。**

## g++
g++是一款使用十分广泛的c++编译器。进行编译输出可执行文件的典型命令如下：
``` bat
g++ -o main main.cpp -std=c++11 -O2
```
以上程序主要的组成为：

- `g++`：指定了使用g++编译器
- `-o`：指定了输出路径，此处命令中为`main`就会输出到当前文件夹中的`main.exe`
- `main.cpp`：指定了输入的源文件
- `-std=c++11`：指定了使用c++11标准编译文件
- `-O2`：指定了使用O2编译优化，编译优化能够在编译中优化代码，加快实际执行速度，最高优化等级为O3但O3存在一些风险，一般使用O2优化，NOIP中不开启编译优化

<br>

使用g++需要注意一下问题：
- NOIP中若要估计实际运行时间，请关闭`-O2`的选项
- 查看帮助请使用`g++ --help`，Unix系习惯`--help`参数为帮助，Windows习惯`/?`

## 函数
函数是一种抽象手段，让人们从重复劳动中解放出来。Batch也能够编写函数，利用`:function`来指定函数的开始，然后利用`call`来进行函数调用和指定参数，函数中第一个参数通过`%1`获取，之后的以此类推，以下是编译函数的实例：
``` bat func_sample.bat
@echo off
call :compile main
pause
goto :eof

:compile
  g++ -o %1 %1.cpp -std=c++11 -O2
goto :eof
```
以上程序和g++一章中的编译指令等价，不过通过更换函数的第一个参数`main`，以上程序能够编译当前目录下的其他的程序。`goto :eof`表示结束，如果不加上程序就会自动执行一遍`:compile`函数，所以主函数和其他函数的后面都需要加上`goto :eof`，最后一个函数后面不用加，不过除非是只有主函数建议加上，之后增加新的函数就不容易出错了。

## 练习
- 尝试在命令行下执行g++编译命令，尝试编译一些错误的程序
- 尝试写一个函数输出第一个参数，更换参数调用几次函数
- 尝试写一个函数输出两个参数的和

# 实用程序的构建

## 检查文件名，测试样例是否通过
NOIP中需要指定文件进行输入输出，一类常见的错误就是文件名输入错误，然后许多时候也可能因为题目输出中的小错误而犯错（大小写，格式错误）。

对于这些错误，除了保持小心谨慎的态度之外还有一种更保险的解决方法就是编写`.bat`脚本进行简单的测试。NOIP中对于每一题`name`，源文件为`name.cpp`，输入文件为`name.in`，输出文件为`name.out`。在此基础上，我们令`name.in`为样例输入文件，并新建一个文件`name.ans`为样例的输出。

以下程序假设有三题分别为noip,noi,ioi则可以构建以下的程序：

``` bat check_name.bat
@echo off

call :run noip
call :run noi
call :run ioi
pause
goto :eof

:run
set file=%1
g++ -o %file% %file%.cpp -std=c++11
%file%
fc %file%.out %file%.ans
goto :eof
```

## 对拍程序

竞赛中常常遇到一些题，小数据可以暴力，大数据使用暴力会超时。这类题目想要保证正确，一种很好的手段叫做对拍，具体来说就是写出大数据的解和暴力解，验证小数据上两个解的输出是否相同。

第一种对拍的方法是将暴力解直接写进大数据的解里面，分成两个函数，直接输出两份答案，肉眼看是否一致，这种方法的好处在于方便，如果错误比较明显的话还是可以很快找出问题的。

第二种对拍的方法是将暴力解写进另一个源文件中，同时构建一个小数据的生成器。使用生成器生成数据让两个程序去跑，不断循环直到两个程序的输出不一致。

以下程序是一个例子，固定`main`为程序名，`gen.cpp`为数据生成器，`brute.cpp`为暴力程序，输出到文件`brute.out`:
``` bat testm.bat
@echo off

set sol=main

call :compile gen
call :compile brute
call :compile %sol%

:loop
gen.exe > %sol%.in
brute.exe
%sol%.exe

fc brute.out %sol%.out > nul
if errorlevel 1 (
	type %sol%.in
	fc brute.out %sol%.out
  pause
	goto :eof
)
goto :loop

:compile
g++ -o %1.exe %1.cpp -std=c++11 -O2
goto :eof
```
给`sol`赋值为其他名字可以测试不同的程序，也可以写成`set sol=%1`然后将程序名作为整个程序的参数传入，比如在命令行中执行`testm main`，`%1`会获得命令行调用的第一个参数。

`n >> 1`和`n / 2`在负数时结果可能不同，可以写以下程序进行验证，稍微跑一会就可以找到错误的例子了：
``` cpp main.cpp
freopen("main.in", "r", stdin);
freopen("main.out", "w", stdout);
int n;
scanf("%d", &n);
printf("%d\n", n >> 1);
```

``` cpp brute.cpp
freopen("main.in", "r", stdin);
freopen("brute.out", "w", stdout);
int n;
scanf("%d", &n);
printf("%d\n", n / 2);
```

``` cpp gen.cpp
srand(time(0));
printf("%d\n", (rand() % 100) - 5);
```

# 资源下载

{% asset_link bat-usage.rar [源码合集下载] %}