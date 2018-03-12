Computer System Concepts

第一章
最常见的多处理器设计是对称多处理 (smp) 特征是:所有处理器是对等关系P2P,运行完全独立于别的处理器

集群系统：特殊形式的多处理器系统，通过局域网连接的多个计算机系统组成

利用CPU:(multiprogramming)多程序
管理让多个jobs同时位于内存来保证CPU总有job to do

分时系统:multiprogramming的一种扩展,CPU使用调度算法不停替换jobs，造成job同时运行的幻觉

为了阻止用户干扰系统正确的操作硬件有两种模式，用户态(mode)和内核态

各种指令有特权只能在内核(kernel)态执行,操作系统所在的内存必须被保护以避免用户的不当修改
timer避免无穷的循环

分布式系统允许地理位置分散的主机通过计算机网络共享资源

云计算不过是分布式系统的另一种称谓

实时操作系统应用于嵌入式环境

第二章
operating systems structures

goals of system be well defined before the design begins

操作系统提供的常见服务:
•用户界面: CLI (Command-line interface) , GUI
•程序执行
•I/O操作途径
•文件管理系统/可能有多种
•进程之间的交流(通过共享内存)
•错误检测(不断的而检测和纠正错误)
•资源分配
• Accounting :跟踪用户使用的计算机资源
•Protection and security:当多个进程同时进行时,进程不应该能够干扰其余进程或者操作系统,应保证所有对系统资源的访问都是可控的


用户操作系统界面:
shell 命令解释器
有两种:
1) 根据所写指令跳转到其代码段,来进行系统调用
2)解释器不懂代码，通过代码寻找到匹配文件，将其加入内存中执行(后期可通过添加file来补充指令)

API(application programming interface):应用编程接口,由系统调用组成,若用系统调用编程会使得编程量提升数量级,因此将其封装集成.

使用API而不是系统调用的原因
1)可移植性
2)工作量减少

3种将参数传给操作系统的方式
1)直接给寄存器
2)参数数量大于寄存器,将参数放在一个块或者TABLE中,把首地址存入寄存器,然后push入栈
系统调用的类型:
System calls can be grouped roughly into six major categories: process
control, file manipulation, device manipulation, information maintenance,
communications, and protection.

debugger—a system program designed to aid the programmer in finding
and correcting errors, or bugs—to determine the cause of the problem

batch system :批处理系统

 If the program discovers an error in its input
and wants to terminate abnormally, it may also want to define an error level

where to return control when the loaded program terminates?
we have effectively created a mechanism for one program to call another program.

In fact, the similarity between I/O devices and files is so great that many operating
systems, including UNIX, merge the two into a combined file–device structure.

memory dump :可帮助软件开发人员和系统管理员诊断，识别和解决导致应用程序或系统故障的问题。

the operating system keeps information about all its processes, and system calls are used to access this information.
System Programs:
• File management. 
• Status information. 
• File modification. 
• Programming-language support. 
• Program loading and execution. 
• Communications. 
• Background services. 

Mechanisms determine how to do something; policies determine what will be done.
