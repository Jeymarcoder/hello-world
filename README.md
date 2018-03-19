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

用高级语言写操作系统可移植性高好写代码还易于维护(汇编逐渐被抛弃)
The only possible disadvantages of implementing an operating system in a
higher-level language are reduced speed and increased storage requirements.

major performance improvements in operating systems are more likely to be the result of better data structures and algorithms than of excellent assembly-language code.

The major difficulty with the layered approach involves appropriately
defining the various layers. Because a layer can use only lower-level layers, careful planning is necessary.
分层系统最大的槽点:
tend to be lessefficient than other types,每一层调用增大系统开销

Linux also uses loadable kernel modules
 hybrid systems (most use)

 Summary:
 System  be classified into several categories: program control, status requests, and I/O requests. 

第三章
process:

only one process can be running on any processor at any instant. Many
processes may be ready and waiting
Process Control Block:(进程控制块)
• Process state. The state may be new, ready, running, waiting, terminated
• Program counter. 
• CPU registers. 
• CPU-scheduling information. 
• Memory-management information
• Accounting information. --->存储CPU和实时使用的数量,时间限制,进程号等
• I/O status information.
PCB是一个仓库来储存进程信息
Threads:(线程)
On a system that supports threads(多线程), the PCB is expanded to include
information for each thread.
3.2 Process Scheduling
不停地为CPU切换进程使其一直处于满负荷状态,提高cpu的利用率
3.2.1 Scheduling Queues:
job queue(processes)
ready queue :进程常驻在主存中切准备完成等待被执行的一个进程列表
device queue:如等待I/O设备请求完成的进程列表(或者别的device)
3.2.2 Schedulers
 in a batch system, more processes are submitted than can be executed immediately.
 so there will be 
 long-term schedulers 也称job schedulers(用于从磁盘中选取那些多余的进程将其调入主存) , 
 short-term schedulers 也称 cpu schedulers(选取主存中ready的进程来执行)
 I/O-bound process (I/O绑定的进程):花在I/O上的时间大于计算
A CPU-bound process :相反
It is important that the long-term scheduler select a good process mix of I/O-bound and CPU-bound processes.
若全部是I/O-bound process,ready queue 会empty,short-term scheduler 没事做 
全部是CPU-bound process ,I/O queue 会empty.
分时系统可能只会有较少甚至没有的long-term scheduler而引入一种medium-term scheduler
3.2.3 context switch
中断产生时会将context存入pcb,不久后又取出执行
definition:Switching the CPU to another process requires performing a state save of the current process and a state restore of a different process. This task is known as a context switch

交换速度取决于memory speed.
highly dependent on hardware support.
3.3 Operations on Processes
1.systems must provide a mechanism for process creation and termination
3.3.1 Process Creation
父进程,子进程 can forming a tree of process
process identifier (or pid): provides a unique value for each process in the system,作为内核访问属性的索引(index)

子进程可直接利用操作系统的资源或者被强制只可使用部分父进程资源-->可以防止因为创建过多的子进程而导致整个操作系统因为过载而宕机
3.3.2 Process Termination
通常exit()的系统调用只能由要终止进程的父进程来调用以避免用户随意避免kill processes
3.4 Interprocess Communication
interprocess communication (IPC):shared memory and message passing /2 fundamental models
就目前来说message passing 对性能的提升会更高,shared memory 受cache一致性的影响
3.4.1 Shared memory
producer–consumer problem Two types of buffers can be used.
The unbounded buffer :即使consumer没消耗完,producer也可以一直生产
The bounded buffer   :2种情况1)当没东西生产出来consumer要等
                            2)当东西生产满了没被消耗producer 要等
producer:
while (true) {
/* produce an item in next produced */
while (((in + 1) % BUFFER SIZE) == out)
;//即空间已满
buffer[in] = next produced;
in = (in + 1) % BUFFER SIZE;
}
consumer:
while (true) {
while (in == out)
; /* do nothing */
next consumed = buffer[out];
out = (out + 1) % BUFFER SIZE;
/* consume the item in next consumed */
3.4.2 Message-Passing Systems
 a communication link:
 direct communication:
 1)需要交流时就自动建立了,但此类需了解identity
 2)连接在2个进程之间
 3)任意两对间都有连接
 indirect communication:
  received from mailboxes
  1)只有有共享邮箱的进程才可建立连接
  2)连接不再局限于2个进程间
  3)每对进程可能会有多个邮箱来交流
  If the mailbox is owned by a process (that is, the mailbox is part of the address space of the process), then we distinguish between the owner (which can only receive messages through this mailbox) and the user (which can only send messages to the mailbox). 
3.4.2.2 Synchronization
Blocking send:The sending process is blocked until the message is
received by the receiving process or by the mailbox
Blocking receive:The receiver blocks until a message is available.
3.4.2.3 Buffering
messages exchanged by communicating processes reside in a temporary queue.
3 种实现方法:
1)零容量:即发出者必须等接受者收到才可继续发.
2)有界容量
3)无界容量
3.6 Communication in Client–Server Systems
3.6.1 Sockets(LOW LEVEL 传递无组织的结构)
all connections consist of a unique pair of sockets.
146.86.5.20:1625----->ip:port

127.0.0.1 is a special IP address known as the loopback 
When a computer refers to IP address 127.0.0.1, it is referring to itself.
This mechanism allows a client and server on the same host to communicate
using the TCP/IP protocol.

port让一个host可以提供多个网络服务通过不同的port
3.6.2 Remote Procedure Calls
Each message is addressed to an RPC daemon listening to a port on the remote system, and each contains an identifier specifying the function to execute and the parameters to pass to that function.
XDR:客户端参数被convert  the machine-dependent data into XDR
    服务端又转回来
how does a client know the port numbers on the server?
1:编译的时候在客户端已有RPC调用已经固定了端口
2:matchmaker,发带有RPC名字的消息给matchmaker,他再提供port
3.6.3 Pipes
the producer writes to one end of the pipe (the write-end)
the consumer reads from the other end (the read-end)
3.6.3.1 Ordinary Pipes:
单向的,只允许单向交流
除了创建pip的进程外别的不可访问
`
if (pipe(fd) == -1) {
fprintf(stderr,"Pipe failed");
return 1;
}/* fork a child process */
pid = fork();
if (pid < 0) { /* error occurred */
fprintf(stderr, "Fork Failed");
return 1;
}if (pid > 0) { /* parent process */
/* close the unused end of the pipe */
close(fd[READ END]);
/* write to the pipe */
write(fd[WRITE END], write msg, strlen(write msg/* close the write end of the pipe */
close(fd[WRITE END]);
}else { /* child process */
/* close the unused end of the pipe */
close(fd[WRITE END]);
/* read from the pipe */
read(fd[READ END], read msg, BUFFER SIZE);
printf("read %s",read msg);
/* close the write end of the pipe */
close(fd[READ END]);
}return 0;
}Figure 3.26 Figure 3.25, continued.
`
3.6.3.2 Named Pipes
双向且无父子进程概念,但不能同时使用
3.7 summary
父子进程allowing concurrent execution: information sharing, computation speedup, modularity, and convenience

第四章 threads
4.1
线程是cpu优化的一个基本单元,包含线程id,pc,寄存器集,栈
同个进程的多个线程共享代码段数据段和文件段
多线程可合理利用多核心处理器的能力
优点:
1)响应性强
2)同个进程的线程资源共享
3)开辟线程比开辟进程经济实惠    
4.2 多线程程序设计
注意并行 parallelism 和同时 concurrent 的区别
并行是多个任务并行执行
同时只是把不同任务在时间片上分块分段跑起来
现代操作系统一个核心支持两个线程
编程需要写调度算法来实现利用多核心的并行运行
数据并行性:同一份计算任务拆成多分同时计算
任务并行性:完全不同的task去并跑
4.3多线程的模式
用户线程:不依赖于内核线程,在程序中实现,且内核线程不知道
内核线程:由操作系统管理
用户线程更易管理和创建因为无内核干涉
1:多(用户线程)对一(内核线程)线程
高效, 但任何一个用户线程造成系统阻塞调用,整个进程宕机
此模式线程无法并行运行,一次只准一个线程访问内核
2:一对一模式
唯一缺点:一个用户线程就会开辟一个内核线程造成的开销性能损耗
通常这种模式系统会限制一个最多的内核线程的数量
Linux,Windows即这种
3:多对多模式:
一般是大量的用户线程对应少量内核线程
理论上最好的模式
既不会因内核线程少导致进程崩溃,也不会开辟过多线程的资源损失
4.5隐式线程
thread pools
线程终结:
1)一个线程去立即终结目标线程
2)周期性的终结




