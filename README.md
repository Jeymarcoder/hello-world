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

```c

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

```

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

```c

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

```

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
<<<<<<< HEAD
2)周期性的终结
第六章 CPU scheduling
1)ready queue is not a FIFO queue  
6.1.3 Preemptive scheduling (抢占型调度)
1)CPU调度只会出现在进程周期的:
  a)从运行队列到终止
  b)从运行队列到等待队列
  c)等待队列到就绪队列
  d)从运行队列到就绪队列  
a和b是无抢占型调度,即轮到进程会让他一直跑完为止.  
The dispatcher is the module that gives control of the CPUto the process selected
by the short-term scheduler. This function involves the following:
• Switching context
• Switching to user mode
• Jumping to the proper location in the user program to restart that program
6.2 Scheduling Criteria(调度准则)
选什么算法来调度要根据实际进程.
调度效果有5个指标
1)CPU 利用率
2) Throughput(吞吐量)
3)Turnaround time (周转时间):提交进程到进程完成的时间
通常来说受到输出设备的速度限制
4)waiting time (等待时延):即ready queue 里的进程进入运行队列所空闲的时间
5)Response time(响应时间):即反馈给用户的时间,是开始可以reponse所需要的时间
6.3 Scheduling Algorithms  
6.3.1 First-Come, First-Served Scheduling(FCFS)
直接使用队列即可(FIFO)在众多算法中会造成最大的等待时延
convoy effect(车队效应):即当只有1个CPU绑定进程和多个I/O绑定进程时,CPU绑定进程会使一辆最大的车,而I/0绑定进程要使用CPU时必须全部等待CPU绑定进程处理完大事件,但是实际上I/O只有很小的事要处理.  
并且,这个算法是 nonpreemptive,非抢占型.此类算法对分时系统影响最大.
6.3.2 Shortest-Job-First Scheduling(SJF)
  appropriate term for this scheduling method would be the shortest-next-CPU-burst algorithm
此算法通常会给出最低的等待时延
in a batch system is used frequently( in long-term scheduling(从队列中按顺序提交.)
short-term CPU scheduling (从众多进程中选一个调入,即无法预测下一个cpu burst 的时长)
即需要一个算法来预测下一个被调入CPU中执行的时长.
Xn+1 = a tn + (1 − a)Xn.  X为预测时长
既可以是抢占型也可以不是
6.3.3 Priority Scheduling
major problem :indefinite blocking, or starvation.
因为优先级过低而一直被滞后
solution:(called Aging) increasing the priority of processes that wait
in the system for a long time.
6.3.4 Round-Robin Scheduling(轮换调度)
designed for time-sharing system
use FIFO queue
time slice, is defined.(时间片) sets a timer to interrupt after 1 time quantum, and dispatches the process.
时间到了立即切换队列中的下一个进程
被动preemptive,
time quantum 太短造成大量的context switches.
             太长了和FCFS就一样了
一般来说大于80%的突发周期即可
6.3.5 Multilevel Queue Scheduling
foreground (interactive) processes and background (batch) processes. These two types of processes have different response-time requirements and so may have different scheduling needs. 
 a multilevel queue scheduling algorithm with
five queues, listed below in order of priority:
1. System processes(最高优先级)
2. Interactive processes
3. Interactive editing processes
4. Batch processes
5. Student processes(最低优先级)
No process in thebatch queue, for example, could run unless the queues for system processes,interactive processes, and interactive editing processes were all empty.
优先级第一
如果低级别的在running 时,有比他高优先级的入队,立即停止并执行.即抢占型
6.3.6 Multilevel Feedback Queue Scheduling
多级队列调度不允许更换队列而这个allows a process to move between queues.
若某进程占据了过多的时间则可以把它调向低优先级队列
同样若某进程等待过久也可以把它提到高优先级
6.4 Thread Scheduling
6.4.1 Contention Scope
process-contention scope (PCS)
the kernel uses system-contention scope (SCS)
6.5 Multiple-Processor Scheduling
SMP和AMP
存在 high cost of invalidating and repopulating caches
load balancing is typically necessary only on systems where each processor has its own private queue of eligible processes to execute.  

###6.6Realtime CPU Scheduling
Soft real-time systems: no guarantee as to when a critical real-time process will be scheduled;
Hard real-time systems have stricter requirements. A task must be serviced by its deadline;  

>Two types of latencies affect the performance of real-time systems:
1. Interrupt latency:中断发生时处理现有信息(如context switch)所需要的时间  
* 导致中断延迟的重要因素之一:内核数据结构更新时中断被禁止导致时延增长  
2. Dispatch latency:the scheduling dispatcher stop one process and start another process  
* main problems:
1. Preemption of any process running in the kernel  
2. Release by low-priority processes of resources needed by a high-priority process

* solution:提供抢占型内核
#第5章**进程同步**
cooperating process:_能影响别的进程也能被别的进程影响的进程_
>T0: producer execute register1 = counter {register1 = 5}
>T1: producer execute register1 = register1 + 1 {register1 = 6}
>T2: consumer execute register2 = counter {register2 = 5}
>T3: consumer execute register2 = register2 − 1 {register2 = 4}
>T4: producer execute counter = register1 {counter = 6}
>T5: consumer execute counter = register2 {counter = 4}  
按照counter++,counter--,本来counter==5但是由于出现进程交叠问题导致错误(data  concurrently)called race condition  
ensure that only one process at a time can be manipulating the variable counter.  
有share memory就会出现这种数据一致性问题  
###*5.2 The Critical-Section Problem*: to design a protocol that the processes can use to cooperate.    
* critical section:计算机中的每个进程的关键代码may be changing the common variables, updating a table so on 这段代码在系统中的重要特征就是no two processes are executing in their critical sections at the same time  又称临界区,是一个访问共用资源的程序片段,但共用资源区不支持被多进程(线程)访问.  
   * entry section: 进程需要提出请求执行critical section的请求的实现followed by an exit section  
   * remainder section:剩余代码
* 3 requirements:
   * Mutual exclusion : 即每次仅一个进程进入critical section
   * Progress : 当没有进程执行critical section时必须要有进程补上进入
   * Bounded waiting :要排队  
Two general approaches are used to handle critical sections in operating systems: preemptive kernels and nonpreemptive kernels.  
###5.3 Peterson’s Solution  
```c++

do {
flag[i] = true;//当前进程i
turn = j;//相对的另一个进程j
while (flag[j] && turn == j);//若没发生交叠flag[j]=false则进入critical section,只要j进程没玩flag[j]=true
critical section
flag[i] = false;
remainder section
} while (true);

```  

turn: 该轮到谁进入关键部分(critical section)
flag: 谁已经准备好了进入critical section
5.4 Synchronization Hardware
###protecting critical regions through the use of locks
one uninterruptible unit. We can use these special instructions to solve the critical-section problem

```c

*********************************
boolean test and set(boolean *target) {
boolean rv = *target;
*target = true;
return rv;
*********************************
do {
while (test and set(&lock))
; /* do nothing */
/* critical section */
lock = false;
/* remainder section */
} while (true);
*********************************
int compare and swap(int *value, int expected, int new value) {
int temp = *value;
if (*value == expected)
*value = new value;
return temp;
}
**********************************
do {
while (compare and swap(&lock, 0, 1) != 0)
; /* do nothing */
/* critical section */
lock = 0;
/* remainder section */
} while (true);
**********************************

```

以上2指令仅满足互斥要求满足不了排队要求
so update test_and_set()
```c

*********************************
do {
waiting[i] = true;
key = true;
while (waiting[i] && key)
key = test and set(&lock);
waiting[i] = false;
/* critical section */
j = (i + 1) % n;
while ((j != i) && !waiting[j])//开始扫描下一个准备好的进程
j = (j + 1) % n;
if (j == i)
lock = false;
else
waiting[j] = false;
/* remainder section */
} while (true);
*********************************

```
##5.5 Mutex(互斥) Locks(不是物理上的是software tools)
process must acquire the lock before entering a critical section
```c

************************************
acquire() {
while (!available)
; /* busy wait */
available = false;;
}
*************************************
release() {
available = true;
}
**************************************
do {
acquire lock
critical section
release lock
remainder section
} while (true);
***************************************

```

实现时会用到lock部分的硬件机制,主要实现缺点:busy waiting called  spinlock  wastes CPU Cycle
                                    优点:no context switch
5.6 Semaphores(信号量)
variable integer S 2 ways :1)wait() 2)signal()

```c++
wait(S) {
while (S <= 0)
; // busy wait
S--;
signal(S) {
S++;

```

且以上两个函数执行时被能被打断
 ###5.6.1 Semaphore Usage: 
Operating systems often distinguish between counting(没有范围) and binary semaphores(可用作mutex locks)
当一个进程想使用资源时就执行wait(), 当一个进程释放资源signal()
使用信号量解决各种各样的同步问题;
如S1和S2 2 个进程且S2必须当S1执行后才可执行:
1)在进程s1中插入语句: S1;
                    Signal((synch));
2)在进程s2中插入语句:wait(synch);
                    S2;
3)设置synch = 0;
5.6.2 Semaphore Implementation
为了克服busy waiting 问题,可以修改wait()函数,当一个进程调用wait时,去查找semaphore的值如果已被用完即把process block丢入一个与 semaphore 有关的队列中,然后控制权就被转入CPU Schduler,当有进程signal()时,应该restart被block的进程,然后扔进ready queue.

```c
//new design of Semaphore

********************************

typedef struct {
int value;
struct process *list;
} semaphore;

********************************

wait(semaphore *S) {
S->value--;
if (S->value < 0) {
add this process to S->list;
block();//基础系统调用

********************************

signal(semaphore *S) {
S->value++;
if (S->value <= 0) {
remove a process P from S->list;
wakeup(P); //restart process P 系统调用

******************************
```

list of waiting processes 可轻易通过在PCB中链接字段实现实现
Each semaphore contains an integer value and a pointer to a list of PCBs.
多处理器必须禁止进程中断
ps:busy waiting并非被解决而是转入critical section, 又因为通常这部分指令都很少所以很少会发生busy waiting problems;
5.6.3 Deadlocks and Starvation
当无限期等待发生时这部分processes就被称作deadlock
设S,Q都被初始化为1
>P0        P1
>wait(S); wait(Q);
>wait(Q); wait(S);
      .       .
      .       .
      .       .
>signal(S); signal(Q);
>signal(Q); signal(S);
如上两个进程相互等待陷入僵局.
Another problem related to deadlocks is indefinite blocking(LILO Queue) or starvation
5.6.4 Priority Inversion
当一个高级别进程想要修改一个正在被低级别进程访问的内核数据时,通常内核会被一个lock保护然后高级别的进程必须等待直至低级别进程使用完成
priority inversion. It occurs only in systems with more than two priorities.如L,M,H 3个级别的进程.当L在访问R资源时H想访问R就进入等待,但此时M启动,他不使用R但是他的高优先级抢占了L,L就挂起了,但H还是在等待L用完R,因此间接影响了H的等待时长.???
solution:priority-inheritance protocol. 
implemention:
如例子中当L访问R资源时,H也要访问R资源,因此L暂时性的继承H的优先级,当L访问完成就恢复L的优先级
5.7 Classic Problems of Synchronization
5.7.1 The Bounded-Buffer Problem:

```c

*********************
/* producer */
*********************
do {
...
/* produce an item in next produced */
...
wait(empty);
wait(mutex);
...
/* add next produced to the buffer */
...
signal(mutex);
signal(full);
} while (true);
***********************

int n;
semaphore mutex = 1;
semaphore empty = n;
semaphore full = 0

*********************
/*consumer*/
do {
wait(full);
wait(mutex);
...
/* remove an item from buffer to next consumed */
...
signal(mutex);
signal(empty);
...
/* consume the item in next consumed */
...
} while (true);
************************

```

变量empty是用生产者生产时看还有多少空位(最开始为n即没有进程进入)
变量full是用消费者看有多少进程完成后离开
5.7.2 The Readers Writers Problem
read database call reader
update database call writer
当多个进程同时read没有问题,但当writer和别的任意进程同时访问database时就有chaos.
2个经典的solution:
1)没有reader会等待除非writer获得了permission来操作shared object
可能会导致writer一直等待
2)没有new reader会开始reading 当有writer在等待进入shared object时
可能会导致reader一直等待

```c

*************************
semaphore rw_mutex = 1;
semaphore mutex = 1;
int read count = 0;
*************************
/*writer structure*/
do {
wait(rw_mutex);
...
/* writing is performed */
...
signal(rw_mutex);
} while (true);

**********************
/*reader structure*/
do {
wait(mutex);
read count++;
if (read count == 1)     //???
wait(rw_mutex);
signal(mutex);
...
/* reading is performed */
...
wait(mutex);
read count--;
if (read_count == 0)
signal(rw_mutex);
signal(mutex);
} while (true);
*****************************

```
适用于readers 比 writers多的应用和容易区分进程only read和only write 的应用
5.7.3 The Dining-Philosophers Problem
每个哲学家一次只能拿起一根筷子,但吃饭要两根筷子.
one simple solutino: make every chopstick a  semaphore.

```c
*******************************
semaphore chopstick[5];//均初始化为1
do {
wait(chopstick[i]);
wait(chopstick[(i+1) % 5]);
...
/* eat for awhile */
...
signal(chopstick[i]);
signal(chopstick[(i+1) % 5]);
...
/* think for awhile */
...
} while (true);
*********************************
```
nevertheless(然而),这会造成deadlock,如果5个哲学家同时饿了,都拿起了左手边的筷子,那么,所有人都会等待右手去拿筷子,陷入deadlock.
Several possible remedies(补救措施):
1:最多允许4个人同时坐在桌上
2:Allow a philosopher to pick up her chopsticks only if both chopsticks are available
3:定义座位奇数(odd number)的先拿左手,偶数(even number)的先拿右手.
beter solution还应该防范饥饿问题


5.8 Monitors
 semaphores 易造成timing errors(主要因为编程时wait()和signal()调用错误会引发严重后果)
 so introduce the high-level synchronization construct—the monitor type .

```c++
******************************
monitor monitor name
{/* shared variable declarations */
function P1(...) {
...
}function P2(...) {
...
}...function Pn(...) {
...
}initialization code (...) {
...
}
}
*******************************
```

The monitor construct ensures that only one process at a time is active
within the monitor.
monitor 中的函数只能访问monitor内部的变量和正式参数
编写一个含有wait和signal的类condition
for example;
when the x.signal() operation is invoked by a process P, there exists a suspended process(暂停进程) Q associated with condition x. Clearly, if the suspended process Q is allowed to resume(继续) its execution, the signaling process P must wait.
conceptually both processes can continue with their execution,2个方案:
1. Signal and wait. P either waits until Q leaves the monitor or waits for
another condition.
2. Signal and continue. Q either waits until P leaves the monitor or waits
for another conditio

5.8.2 Dining-Philosophers Solution Using Monitors
imposes(施加) the restriction:
philosopher may pick up her chopsticks only if both of them are available.

```java

****************************************
monitor DiningPhilosophers
{
  enum {THINKING, HUNGRY, EATING} state[5];
  condition self[5];
  void pickup(int i) {
  state[i] = HUNGRY;
  test(i);
  if (state[i] != EATING)
  self[i].wait();//进入waiting
  }
  void putdown(int i) {
  state[i] = THINKING;
  test((i + 4) % 5);
  test((i + 1) % 5);
  }
  void test(int i) {
  if ((state[(i + 4) % 5] != EATING) &&(state[i] == HUNGRY) &&(state[(i + 1) % 5] != EATING)) {
  state[i] = EATING;
  self[i].signal();
    } 
  }
  initialization code() {
  for (int i = 0; i < 5; i++)
  state[i] = THINKING;
  }
}
*****************************************
```

由于新增的限制所以:
i can set the variable state[i] = EATING only if her two neighbors are not eating: (state[(i+4) % 5] != EATING) and (state[(i+1) % 5] != EATING).
以上实现还是有可能造成饥饿.
   *                        *<---
 *<-  *<-                *     * 
  * *                     *<- *
如上2对交替eating造成剩余的那个一直饥饿
5.8.3 Implementing a Monitor Using Semaphores
external function F

```c++
int next  = 0;
semaphore  mutex = 1;
semaphore  x_sem = 0;
int x_count = 0;//count the number of processes suspended on next
int next_count = 0;
int main(){ 
  wait(mutex);
...
  body of F
...
  if (next_count > 0)
  signal(next);
  else
  signal(mutex);
}
wait(){
x_count++;
if (next_count > 0)
signal(next);
else
signal(mutex);
wait(x_sem);
x_count--;
}
signal(){
  if (x_count > 0) {
  next_count++;
  signal(x_sem);
  wait(next);
  next_count--;
  }
}
//??????
5.8.4 Resuming Processes within a Monitor

