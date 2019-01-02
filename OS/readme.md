# 操作系统 OS

## 操作系统将程序加载进入内存, 在运行这个程序成为一个新的进程的时候, 如何限制这个程序内的一些操作 ?

引入一个新的进程模式, 用户模式 user mode

硬件提供给程序系统调用的能力,

操作系统在 boot 的时候, 会让 hardware 记住一张 trap table. 上面会写当程序发起陷阱指令, 硬件应该去调用OS哪个处理方法 (trap handler).

提供的trap table 会有handler的地址

## 每个进程是否都有相应的 kernel stack 用来存放 register ?

是的

甚至每个线程也有自己的内核栈

线程和进程的唯一区别就是 多个线程可以共享同一个地址空间

## OS 是一个 process 吗?

不是

内核不是一个进程, 是一个程序

## OS 启动了 timer 用于 interrupt, 但是把 CPU 控制权给了 user program 以后, 这个 timer 还会运行吗 ?

运行的timer是一个高优先级的操作

OS会让硬件一直运行 timer

## OS 在硬件的支持下如何将虚拟地址转化为物理地址 ?

物理内存中会存在一张匹配当前 process 的 page table

通过VPN, 在 page table 中找到 PFN, PFN 和 offset 组合使用找到对应的物理地址

## 什么是 TLB ? 全称是什么 ?

*todo*

## MMU 是什么 ?

内存管理单元 (memory management unit)

负责处理中央处理器 (CPU) 的内存访问请求的硬件

功能:

- 虚拟地址 转换为 物理地址 (虚拟内存管理)
- 内存保护
- CPU高速缓存的控制

## 为什么使用 TLB 会提升性能 ?

不需要到拥有所有 translation 的 page table 中去查询

## 为什么 TLB 只对当前运行进程有用 ?

每一个进程的虚拟地址都是从相同的地址开始

每次切换进程, page table 都会被全部重新替换

## 什么情况下会发生 page fault ?

当加入了 swap space 机制之后, 当 hardware 在 page table 中找寻到对应的 PTE 以后, 想要根据 PTE 搜寻 page 时, 对应的 page 不一定在 物理内存中.

hardware 可以通过 PTE 上的 present bit 可以知道, 对应的 page 是否存在于 物理内存 中.

如果 present bit 置为 1, page 处于 物理内存中; 如果 present bit 置为 0, page 不处于物理内存中, 而在 disk 或者其他地方.

如果访问了不在物理内存中的 page, 会产生 page fault (页错误)


## 每个设备可能有自己的简单的 CPU, 内存, 以及硬件特定的芯片, 对吗 ?

对

## time sharing 和 space sharing 有什么区别 ? 时分和空分有什么区别 ?

time sharing, 资源(比如CPU)被一个实体(比如进程)使用一会, 然后被另一个实体使用一会.

space sharing, 将资源在空间上分成多份. 比如 disk.

## 进程由哪些部分组成 ?

- 内存 (地址空间)

- 寄存器 (包括 程序计数器PC, stack pointer ...)

- 程序 和 数据

## OS 如何让一个程序转换为进程 ?

程序会以可执行的格式, 存储在 disk 中, OS 首先需要将程序代码和静态数据, 加载到内存, 加载到进程的地址空间

代码和静态数据加载到内存中以后, 在运行进程之前, 需要给程序分配 stack (stack 可用于本地变量, 函数参数, 返回地址的使用),

OS 可能会给程序分配 heap (heap 用于动态的分配内存),

OS 还会将程序关联 input/output.

最后, OS 会从程序的入口开始运行程序, main() 函数, OS 将 CPU 转给新创建的进程

## 如果进程进入了 blocked 状态, 什么时候会解除 ?

如果进程进入了 blocked 状态, 需要等待特定事件的发生才有可能进入 ready 状态

## process 有哪些 state ?

进程存在三种状态:

[![png.jpg](https://i.postimg.cc/KzCGf3bK/png.jpg)](https://postimg.cc/0Md12Qbx)

- 运行: 该时刻进程实际占用着CPU
- 就绪: 可运行, 但因为其他进程占用着CPU而暂时停止
- 阻塞: 除非某种外部事件发生 (比如 I/O 请求完成), 否则进程不能运行 (比如进程向 disk 请求 I/O 操作, 此时进程会进入 blocked, OS 调度其他进程运行)

转换:

1. 进程因为等待输入而被阻塞

当一个进程在逻辑上不能继续运行, 它就会被阻塞

2. 调度程序选择了另一个进程

3. 调度程序选择了 该 进程

4. 出现有效的输入

## 进程只有三个状态吗 ?

进程主要有上面提到的三种状态, 还有其他一些状态, 比如 initial state, 用于创建进程; 比如 final state, 进程退出, 但是还没有被清除

## 如何提高CPU (资源) 的利用率 ?

当运行中的进程发起了 I/O 请求而进入了 blocked 状态, OS 需要调度另一个 ready 的进程进入 running 状态, 从而保持 CPU 的忙碌, 提高资源的利用率

## OS 是一个程序 ?

对, 也拥有对应的数据结构

比如, OS 需要维护进程表, 跟踪正在运行的进程, 跟踪 blocked 的进程

## OS 维护的进程表是怎样的 ?

process list 会包含 OS 中的所有进程

进程列表中的每一项叫做 process control block (PCB).

## PCB 包含什么内容 ?

*todo*

## 发生 process 阻塞有哪些情况 ?

- 向 disk 发起 I/O 请求

- 等待从网络来的 packet

## PID 是什么 ?

全称是 process identifier

在 UNIX 系统中, PID 是用于给 process 命名的. process 的唯一标识

## fork() 的运行机制是怎样的 ?

调用 fork() 之后, 会复制一个与父进程完全一样的进程,

然后父子进程, 都会从 fork() 处进行 return.

子进程不会从 main() 开始运行, 会从 fork() 返回并开始运行

调用 fork() 以后, 父子进程有自己的地址空间, (也就是有自己的内存), 有自己的寄存器, 自己的PC,

不过它们从 fork() 返回的值是不一样的

## int rc_wait = wait(NULL); 传入NULL代表什么 ?

*todo*

## exec() 的运行机制是怎样的 ?

执行 exec() 会加载可执行程序的代码, 覆盖当前的 code segment, 以及当前的 static data.

heap 和 stack 以及内存空间的其他部分也会重新初始化, 不会创建一个新的 process, 而是将当前进程中运行的程序进行转换

exec() 调用运行成功不会返回

## 为什么这样设计API ? fork() 和 exec()

允许在调用 fork() 之后, 在调用 exec() 之前, 运行一些特定的代码,

这部分特定的代码可以改变准备运行的程序的环境

fork 和 exec 分开, 可以实现 input/output 的重定向, pipes ...

### 举例: wc p3.c > newfile.txt

当子进程创建以后, 在调用 exec() 执行 wc 程序之前, shell 关闭标准输出, 打开文件 newfile.txt

于是, 任何从 wc 程序输出的内容会被输入到文件中





















