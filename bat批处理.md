## bat批处理命令

#### dir /?

查看dir命令的使用方法

#### dir

显示

#### cd

显示当前目录名或改变当前目录，切换文件夹

#### ver

显示 Windows 版本

#### time

显示或设置系统时间

#### date

显示或设置日期

#### cls

清除屏幕

#### pause

暂停批处理程序，并显示： `请按任意键继续...`

#### D：

切换至D盘

#### copy

将一份或多份文件复制到另一个位置

```
copy 1.exe 2.exe	复制文件'1'到当前目录并命名为'2'
```

#### move

移动文件并重命名文件和目录

```
move abc bc		将名为'abc'的文件移动至当前目录并重命名为'bc'
```

#### del

删除一个或多个文件

```
del bc		删除'bc'
```

#### md(mkdir)

创建目录

```
md abc 		创建一个名为'abc'的文件夹
```

#### rd

删除一个目录

```
rd bc /S		删除名为'bc'的文件夹并删除其所有子项
```

#### ren

重命名文件

```
ren abc def		将名为'abc'的文件重命名为'def'
```

#### xcopy

复制文件和目录树

```
xcopy notes \abc 	将'notes'目录下的文件复制到'abc'目录中
```

#### echo

显示消息，或者启用或关闭命令回显

```
@echo off
```

#### %temp%

环境变量

```
echo %temp%		输出环境变量具体路径
dir %temp%		环境变量目录列表
```

#### set

显示、设置或删除 cmd.exe 环境变量

#### if

执行批处理中的条件处理

```
if exist d:\notes (echo 12345) else echo 00000	判断 d:\notes文件是否存在，如果存在输出12345，反之是00000
```

#### goto

将 cmd.exe 定向到批处理程序中带标签的行

```
set /P x= 
echo %x%
if %x%==a goto aaa
if %x%==b goto bbb

:aaa
echo 运行到了：aaa
goto end

:bbb 
echo 运行到了：bbb
goto end

:end
pause
```

#### start

启动一个单独的窗口以运行指定的程序或命令 （可以开启多个进程）

#### call

在批处理程序中调用另一个批处理程序

```
call :sub hello world
echo 运行到了这里		sub运行完成后会回到此处继续运行后边的程序
pause
exit

:sub
echo 运行到了子函数
echo %1
echo %2
```

```
主文件
call sub.bat hello world
echo 运行到了这里
pause
exit
```

```
子文件
echo 运行到了sub.bat
echo %1		参数1
echo %2		参数2
```

#### >、>>

屏幕窗口内容输出到文件，> 将窗口内容输出到指定txt文件中，只保存最新一次打印内容，>> 将窗口内容输出到指定txt文件中，可叠加保存打印内容

#### 通配符 ？*

？可以替代一个字符，* 可以替代一串字符

#### for

对一组文件中的每一个文件执行某个特定命令

```
在 cmd 窗口中： for %I in (command1) do command2
在批处理文件中： for %%I in (command1) do command2

for %%i in (*.*) do echo %%i	%%i：变量名  *.*：变量名、文件名
```

#### 快捷键

F7：显示之前输入的命令，上下键切换，右键选择

tab： 自动补全，匹配多个时循环补全

 #### find

在文件中搜索字符串

```
find "333" data.txt
```

#### findstr

在文件中寻找字符串

```
findstr /r "[1-9]" data.txt
findstr /r "[a-z]" data.txt
```

#### 管道符 |

将 | 前边输出的值传给后边的命令

```
ipconfig | find "IPv4"
dir | find ".jpg"
```

#### type

显示文本文件的内容

```
type data.txt
type data.txt | find "a"
```

#### 命令连接符 &  &&  ||

& 前面命令执行后接着执行后面的命令

&& 前面的命令执行成功后才执行后面的命令

|| 前面的命令执行失败的时候才执行后面的命令

```
echo 123 & echo 456 & echo 789
```

```
copy 6789.txt araer.txt && echo 123		6789.txt不存在，那么便不会打印123
```

```
echo 123 & echo 456 & echo 789			打印123不会出错，所以只会打印123
copy 6789.txt araer.txt || echo 123		前面失败，所以会打印123
```

#### fc

比较两个文件或两个文件集并显示他们之间的不同

```
fc 1.txt 2.txt
```

#### comp

比较两个文件或两个文件集的内容

```
comp 1.txt 2.txt
```

#### clip

将命令行工具的输出重定向到 window 剪切板。这个文本输出可以被粘贴到其他程序中

```
type data.txt | clip
clip < data.txt
```

#### 让电脑发出提示音

```
echo ^G				^G: Ctrl+G
```

#### title

设置命令提示窗口的窗口标题

```
title 1234567890
```

#### color

设置默认的控制台前景和背景颜色

```
color 0f
```

#### help

提供 Windows 命令的帮助信息

#### more

逐屏显示输出

```
help | more
```

#### assoc

显示或修改文件扩展名关联

#### attrib

显示或更改文件属性

#### ftype

显示或修改用在文件扩展名关联中的文件类型

#### label

创建、更改或删除磁盘的卷标

#### convert

将 FAT 卷转换为 NTFS

#### format

格式化磁盘以供 Windows 使用

#### chkdsk

检查磁盘并显示状态报告

#### chkntfs

启动时显示或修改磁盘检查

#### print

打印文本文件

#### path

为可执行文件显示或设置一个搜索路径

```
path %path%;c:\		不替换原有的路径，在后面添加c:\路径
path c:\			将原有路径替换成c:\路径
```

#### subst

将路径与驱动器号关联（映射）

#### mklink

创建符号链接

#### compact

显示或改变 NTFS 分区上文件的压缩

