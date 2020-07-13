# gdb调试
# 1.调试原理
## 1.1符号表
- 级别一,有源码文件  
- 级别二,有行号信息,有入参信息
- 级别三,strip -d 可执行文件,没有行号,没有入参
- 级别四,strip -s 可执行文件,没有函数名,字符串表
# 2.调试操作
## 2.1gdb调试三种方式
* gdb打开可执行文件
* gdb coredump
* gdb attach 正在运行的文件

### 2.1.1gdb打开可执行文件
gdb --args 可执行 ...  
gdb 可执行  
默认参数 -se [*-symbols -exec*]
* 此时程序还没有运行处于t状态
* r指令 程序开始运行
* start指令 程序开始运行,停在main函数
### 2.1.2gdb打开coredump
gdb -c dump文件  
**当gdb正在调试一个运行的文件,产生dump文件后,会直接进入dump信息**

### 2.1.3gdb负载到正在运行文件
gdb -p pid
gdb attach pid
进入gdb程序后 attach pid  
* 此时程序所有线程会被打断进入t状态
detach 退出负载

# 3.操作技巧
## 3.1执行外部bash的命令
* shell + 命令  
* shell   **直接转到bash**
* exit   **返回**

# 4.gdb 调试命令
## 4.1printf命令
### 4.1.1格式输出
* /x 十六进制
* /o 八进制
* /t 二进制
- /d 有符号整数,大于四字节ld
- /u 无符号整数,大于四字节lu
- /f 浮点数
* /c 字符
* /s 字符串
## 4.2查看堆栈地址
* p $sp
## 4.3查看内存
* x /nfu 内存地址
- n是几个单元
- f是格式
- u是单元大小 d是四字节g是八字节
## 4.4list命令
* l [func] 显示文件名,行号
## 4.5打断点break
* b [函数名]
* b [file]:[行号]
* b *[地址]
### 4.5.1条件断点
* break [func] if cond
## 4.6反汇编
* 反汇编命令disassemble<简写disas>
* disassemble + [地址]
* disassemble + 函数名  
*参数/m加源文件信息*
## 4.7指定源码目录
* -cd 启动可执行文件参数
* directory 启动gdb之后

## 4.8多线程调试
### 4.8.1操作其他线程
thread apply {all|[0-],[0-]...} 命令
### 4.8.2多线程调试模式
**all-stop模式和no-stop模式（gdb7.0之前不支持no-stop模式）**
* all-stop 模式
在这种模式下，当你的程序在gdb由于任何原因而停止，所有的线程都会停止，而不仅仅是当前的线程。一般来说，gdb不能单步所有的线程。因为线程调度是gdb无法控制的。无论什么时候当gdb停止你的程序，它都会自动切换到触发断点的那个线程。
* no-stop模式（网络编程常用）
### 4.8.3设置no-stop模式操作
* 要在程序调试前设置如下命令
```
#Enable the async interface
(gdb) set target-async 1
#If using the CLI, pagination breaks non-stop
(gdb) set pagination off
#Finall, turn it on
(gdb) set non-stop on
```
* gdb 支持的命令有两种类型：前台的（同步的）和后台（异步 ）的。区别很简单，同步的在输出提示符之前会等待程序 report 一些线程已经终止的信息，异步则是直接返回。所以我们需要 set target-async 1
* set pagination off 可以不让线程结束的时候出现 Type < return > to continue 的提示信息
### 4.8.4是否锁定其他线程
* set scheduler-locking off|on|step
```
off：不锁定任何线程，默认值。
on：锁定其他线程，只有当前线程执行。
step：在 step （单步）时，只有被调试线程运行。
```
# 5.信号
## 5.1信号屏蔽
## handle SIGxx nostop noprint 
# 6.gdb脚本与gdb参数
