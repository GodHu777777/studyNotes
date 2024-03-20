# Ch1 Intro to OS

## Basic Structure

![structure](https://s2.loli.net/2024/02/28/G3yoealAudHPJZs.jpg)

## Simple Batch Processing<a name="batch"></a>

![](https://s2.loli.net/2024/03/05/68qiRb5ns4gCv9L.png)

Q: 图中怎么体现单道?

A: Given the high cost of the equipment, it is not surprising that people quickly looked for ways to reduce the wasted time. The solution generally adopted was the **batch system**. The idea behind it was to collect a tray full of jobs in the input room and then read them onto a magnetic tape using a small (relatively) inexpensive computer, such as the IBM 1401, which was quite good at reading cards, copying tapes, and printing output, but not at all good at numerical calculations. Other, much more expensive machines, such as the IBM 7094, were used for the real computing. This situation is shown in Fig. 1-3.

After about an hour of collecting a batch of jobs, the cards were read onto a magnetic tape, which was carried into the machine room, where it was mounted on a tape drive. The operator then loaded a special program (the ancestor of today’s operating system), which read the first job from tape and ran it. The output was written onto a second tape, instead of being printed. After each job finished, the operating system automatically read the next job from the tape and began running it. When the whole batch was done, the operator removed the input and output tapes, replaced the input tape with the next batch, and brought the output tape to a 1401 for printing **off line** (i.e., not connected to the main computer).

## Time-Sharing System

1. time slice

   A short interval of time allotted to each user or program in a multitasking or timesharing system.



## Interrupt & Context Switch

![](https://2516195395-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MEmUhsvIFBb8ar2-Um3%2F-MErIRhnYslOcH-BziJH%2F-MErLuXL97y8EhjugqGd%2F%E4%B8%AD%E6%96%AD%20%26%20%E5%BC%82%E5%B8%B8.svg?alt=media&token=f7a34956-4ae9-4a11-8652-92889944c950)

![](https://s2.loli.net/2024/02/28/XvJhP7oGROclyiw.png)



**System Calls are the only way through which a process can go into kernel mode from user mode.**

> https://www.geeksforgeeks.org/user-mode-and-kernel-mode-switching/



![System Call](https://2516195395-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MEmUhsvIFBb8ar2-Um3%2F-MErhJqxCoyHvmlZwfi0%2F-MErjHKymHytfwQPLibi%2F%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8.svg?alt=media&token=87cb702d-556b-49d4-94f1-ef11d1696755)

# Ch2 Job Management and User Interface

## 2.1 作业的概念

作业：就是用户一次请求计算机系统为他完成任务所进行的工作总和。

job: a program or set of programs(from Modern Operating System)

作业的组成：包括程序、数据、作业控制信息。

The structure of a typical input job is shown in Fig. 1-4. It started out with a \$JOB card, specifying the maximum **run time** in minutes, the account number to be charged, and the programmer’s name. Then came a \$FORTRAN card, telling the operating system to load the FORTRAN compiler from the system tape. It was directly followed by the program to be compiled, and then a \$LOAD card, directing the operating system to load the object program just compiled. (Compiled programs were often written on scratch tapes and had to be loaded explicitly.) Next came the \$RUN card, telling the operating system to run the program with the data following it. Finally, the \$END card marked the end of the job. These primitive control cards were the forerunners of modern shells and command-line interpreters.



![Job compose](https://s2.loli.net/2024/03/05/q6ajyr8cZ4JUDXI.png)



## 2.2 作业的处理过程

作业控制块(Job Control Block): 作业控制块是作业存在的唯一标志，是系统为管理作业所设置的一个数据结构。每个作业进入系统，系统为其建立JCB。

## 2.3 作业的输入/输出方式

1. [off line](#batch)

2. [Spooling](https://www.codingninjas.com/studio/library/what-is-spooling)(Simultaneous Peripheral Operation On Line)

<img src="https://s2.loli.net/2024/03/05/AHnSCYxKli5BwIM.png" alt="spooling" style="zoom:50%;" />

## 2.4 三级调度

1. 高级调度：作业调度
2. 中级调度：对换调度
3. 低级调度：进程调度

#### 作业调度算法评价

作业$J_i$提交时间为$t_{si}$，i.e. 从后备队列到内存的时刻

开始时间：第一次执行的时刻

执行时间为$t_{ri}$，完成时间为$t_{oi}$

单个作业周转时间 $T_i = t_{oi} - t_{si}$

单个作业带权周转 时间 $W_i = T_i / t_{ri}$

$n$个作业的平均周转时间$T$和平均带权周转时间（周转系数）$W$分别为
$$
T = \frac1n\sum_{i = 1}^n{T_i} \\
W = \frac1n\sum_{i = 1}^n{W_i}
$$


1. 先来先服务(FCFS)

2. 短作业优先调度算法(SJF)

3. 最高相应比优先调度算法(HRP)

   1 + 作业等待时间(当前时刻 - 作业提交时刻$t_{si}$) / 作业估计运行时间(执行时间$t_{ri}$)



# CH3 进程管理

## 3.1 进程的引入

单道批处理：

1. 顺序性：CPU严格按照程序结构所指定的次序执行。
2. 封闭性：独占全部资源，资源的状态只能由该程序本身改变，不受其它程序和外界因素影响。
3. 可再现性：如果程序执行环境和初始条件相同，则其执行的结果相同。

多道批处理：

1. 失去封闭性：共享资源，程序之间互相制约。
2. 间断性：程序之间的制约关系致使程序执行时间不连贯。
3. 不可再现性：失去封闭性，也就失去了可再现性，程序执行的结果随速度、环境的不同而不同。

### 3.2 进程定义

进程是程序的一次执行

进程组成：有程序段、数据段和进程控制块(PCB)组成。

## 3.3 进程状态

1. 新建态（New）：已完成创建一进程所必要的工作– PCB。但尚未同意执行该进程 – 因为资源有限
2. 就绪态（Ready）：已经获得所需资源，只等待CPU
3. 运行态（Running）：进程已经获得所需资源，并占有CPU
4. 阻塞态（Blocked）：也称为等待态、睡眠态等，进程等待某个事件，如等待I/O完成，等待某个资源
5. 终止态（ Terminated ）：终止执行后， 进程进入该状态；可完成一些数据统计工作；资源回收：归还内存，删除PCB

![](https://s2.loli.net/2024/03/17/gIOQECiRt36r7kZ.png)

挂起 Suspend

	1.  就绪挂起
	1.  阻塞挂起

## 3.4 进程控制



