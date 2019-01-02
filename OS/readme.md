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


















