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

### 举例:

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
#include <fcntl.h>

int main() {
  int rc = fork();
  if (rc < 0) {
  } else if (rc == 0) {
    close(STDOUT_FILENO);
    open("./p4.output", O_CREAT|O_WRONLY|O_TRUNC, S_IRWXU);

    char * myargs[3];
    myargs[0] = strdup("wc"); // program: "wc" (word count)
    myargs[1] = strdup("CMakeCache.txt"); // argument: file to count
    myargs[2] = NULL; // marks end of array
    execvp(myargs[0], myargs);

  } else {
    int rc_wait = wait(NULL);
  }

  return 0;
}
```

#### 例子中, `close(STDOUT_FILENO);` 作用是什么 ? `open("./p4.output", O_CREAT|O_WRONLY|O_TRUNC, S_IRWXU);` 作用是什么 ?

close() 和 open(), 还有 fclose() 和 fopen()

fclose() 和 fopen() 是与 file stream 有关的函数, 需要将 stream 比如 `FILE *ptr` 传入函数

close() 和 open() 是与 file descriptor 有关的函数, 需要将 descriptor 比如 `int fd` 传入函数

fclose() 和 fopen() 是 C 标准函数, 因此是可移植的, 而 close() 和 open() 是 POSIX-specific 的函数, 是不可移植的.

*todo*

## POSIX 是什么 ?

是 Portable Operating System Interface for uni-X 的缩写

可移植操作系统接口

为各种UNIX系统定义的一套统一的API标准

## `kill()` 系统调用的运行机制是怎样的 ?

kill() 方法用于发送信号给process.

包括暂停, 杀死以及其它有用的指令.

在大部分 UNIX 终端, 会结合键盘进行发送特殊命令给当前process

比如 control-c 发送 SIGINT (中断)

control-z 发送 SIGTSTP(暂停)

## 一个 user 会不会发信号给另一个 user 的 process ?

不会, 除非是 root

在 user 的权限下会运行很多process, 该 user 只能给他的 process 发送信号

在同一台 PC 上, OS 会给每个 user 分配对应的 CPU, 内存, disk.

## ps 命令

ps 列出正在运行的进程

## top 命令

展示系统的进程, 以及进程使用了多少 CPU 或其他资源

## 进程间的变量不共享 ?

[![image.png](https://i.postimg.cc/9fMwN961/image.png)](https://postimg.cc/BjrvjjQ1)

```
输出:
child x: 200 
parent x: 0 
parent x: 100 
```

子进程有一套自己的变量, 是父进程的副本

## exec() 的变体: execl(), execle(), execlp(), execv(), execvp(), execvpe() ?

execl, execlp, execle, execv, execvp, execvpe - execute a file

方法:
```
#include <unistd.h>
extern char **environ;
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[], char *const envp[]);
```

这些函数的第一个参数是需要执行的文件的名字

在 execl, execlp, execle 函数中,

`*arg, ...` 可以作为 arg0, arg1, ..., argn, 也就是传递给 file 的参数, 第一个参数 arg0 通常是 file

参数列表通常以一个 NULL 指针结束

execv, execvp, execvpe 函数提供的是一个参数数组, 与上面情况类似, 第一个参数是 file, 最后一个参数是 NULL:

[![image2.png](https://i.postimg.cc/T2q2gwSP/image2.png)](https://postimg.cc/NLFwwgQh)

execle, execvpe 都带有 e, 可以通过参数 envp 指定环境变量.

envp 是一个字符串数组, 最末尾一个参数需要是 NULL.

## execlp, execvp, execvp 函数, 第一个参数是 file, 其他是 path, file 和 path 有什么不同 ?

参数为 file:

如果指定的文件名不包含斜杠 ( / ) 字符,

则execlp, execvp和execvpe函数会根据 在PATH环境变量中指定的以冒号分隔的目录路径名列表中查找该文件, eg: (PATH 环境变量是 "/bin:/usr/bin", 则查找 /bin 和 /usr/bin)

如果未定义此变量, 则路径列表默认为当前目录, 后跟 confstr 返回的目录列表 (\_CS\_PATH)

如果指定的文件名不包含 /, 则不会去查找 PATH 环境变量, 根据 file 给出的路径查找文件

## 如果在子进程中使用 wait() ?

wait() 返回 -1

## waitpid 和 wait 区别 ? 什么时候用 waitpid ?

概要:

```C
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait(int *status)
pid_t waitpid(pid_t pid, int *status, int options);
int waitid(idtype_t idtype, id_t id, siginfo_t *infop, int options);
```

功能:

用于等待进程改变状态

描述:

以上方法用于父进程等待子进程, 当子进程状态改变, 父进程获得对应的信息.

状态改变也就是发生了以下情况之一: 子进程结束, 子进程因为信号而停止, 子进程被信号重新唤醒.

如果子进程的状态已经发生过变化, 则这些调用会马上返回. 否则会一直阻塞, 直到子进程状态发生改变, 或者信号 handler 中断调用.

wait() 和 waitpid():

wait() 会挂起正在执行的进程, 直到子进程终止, wait(&status) 相当于 waitpid(-1, &status, 0).

waitpid() 会挂起正在执行的进程, 直到指定 pid 子进程 状态发生改变. 默认情况下, waitpid 等待直到子进程终止, 但是可以等其它状态改变, 在 options 参数内进行修改.

在 waitpid 中, pid 值可以为:

< -1: 等待任意一个子进程, 子进程的进程组id与 指定pid的绝对值 一致

-1: 等待任意一个子进程

0: 等待任意一个子进程, 子进程的进程组id 与调用进程的进程组id 一致

> 0: 等待指定一个子进程, 进程号为 pid

## `int rc_wait = wait(NULL);` 传入NULL代表什么 ?

wait(&status), 即不关心 status

## pipe() 的运行机制 ?

接口:
```C
#include <unistd.h>
int pipe(int pipefd[2]);
```

描述:

创建一个 pipe, 单向的, *用于进程间的通信*.

传入的参数数组 pipefd 用于返回引用 pipe 的末端的两个 file descriptors,

pipefd[0] 引用 pipe 末端的 read, pipefd[1] 引用 pipe 末端的 write.

## 为什么要使用 `exit(EXIT_FAILURE);` 退出 ? 而不是 `exit(0);` ?

`EXIT_FAILURE` 是 `stdlib` 中的整数常量

如果是正常进程终止, 使用 `EXIT_SUCCESS` 或者 `EXIT_FAILURE` 比使用 1, -1, 0 更具有可移植性

## `perror("pipe");` 有什么作用 ?

perror - print a system error message

## `_exit(EXIT_SUCCESS);` ? 为什么要加 `_` ?

terminate the calling process

当子进程里面调用 exec() 失败, 应该调用 `_exit()` 来退出子进程

其他情况使用 exit() 就行

## OS 如何执行一段 program (direct execution / 直接执行) ?

OS在 process list 中创建一条 process entry,

为程序分配一些内存,

从 disk 从加载程序到内存中,

定位程序的入口 main() 然后进行跳转,

然后开始运行用户代码.

但是, 像上面是直接运行程序, 那么: 如何确保程序不会做其他出格的事, 比如访问不可访问的内存 ?

## OS如何停止正在运行的程序, 切换到其他 process, 从而实现 time sharing ?

*todo*

## 如何为 process 添加限制 ?

引入了新的 processor mode (处理器模式): user mode

在 user mode 下, process 不能发起 IO 请求, 不然处理器会抛错, OS 会 kill 掉这个 process

OS运行在 kernel mode, 代码可以运行任何操作

## user mode 和 kernel mode 是谁在维护的 ?

hardware 维护

在 user mode 下, 应用对 hardware 资源没有全部访问权

在 kernel mode 下, OS 对 hardware 的所有资源都有访问权

hardware 会提供一些指令, 包括:

trap into kernel(陷入内核),

return-from-trap 返回到 user mode 程序,

OS 通过 hardware 提供的指令告诉 hardware, trap table 在内存中的位置.

## trap 指令, 任何 user program 都可以执行吗 ?

目前为止看来, 是的

*todo*

## kernel 就是 OS 吗 ?

是的

trap into kernel (trap into OS)

*todo*

## return-from-trap 指令是 OS 发起的 ?

那么 OS 就需要知道 trap 指令是否完成,

OS 处于 kernel mode 中

## hardware 执行 trap 指令之前, 谁为调用者保存 registers ? 保存到哪里 ?

在 x86中, CPU (hardware) 将 program 的 pc, flags, 其他registers, push 到每个进程的 kernel stack.

而 OS 发起的 return-from-trap 指令, 会将 stack 中的值 pop 出, 并恢复 program 的运行.

## 什么是进程的 kernel stack ?

每一个 process 都有一个 kernel stack.

当 hardware 在 user mode 和 kernel mode 之间切换的时候, 用于存储和恢复 register,

kernel stack 由 hardware 维护

## 当系统启动, OS 会告诉 hardware, 当发生确定的 exceptional event, 需要运行哪些代码 ?

在系统启动的时候, kernel 会建立一个 trap table

这张表里面, OS 告诉 hardware, 当发生特定的 event, 需要执行的 trap handlers 的地址是什么

系统 reboot, 会重新建立 trap table

## 有哪些 exceptional event ?

比如:

hard disk interrupt,

keyboard interrupt,

program 发起一个系统调用

## 发起指令告诉 hardware, trap table 在哪里, 也是一个高优先级操作吗 ?

是的, 不能在 user mode 中发起该指令

## 运行在 user mode 下的 process, 如何发起 IO 请求 ?
## user mode 下 program 调用 trap 指令陷入内核模式以后, 是将 CPU 切换给了 OS 吗 ?

[![image3.png](https://i.postimg.cc/QtnM9nVr/image3.png)](https://postimg.cc/qhyrWw8m)

当 user program 发起 系统调用, trap into kernel (trap into OS), 会调用到 hardware 的指令,

hardware 保存 user program 的 register 到 kernel stack,

hardware 切换到 kernel mode,

hardware 跳转到对应的 trap handler 执行代码,

trap handler 是 OS 中的代码, 因为现在处于 kernel mode, 所以 OS 可以进行对应的 syscall, 比如 IO 操作

OS 完成 syscall (system call) 以后, 调用 return-from-trap 指令,

hardware 从 kernel stack pop 出 register, 切换回 user mode, 返回原来的 PC (program counter),

user program 继续执行,

当 user program 执行完, 会调用 exit(), trap into OS

## hardware 也就是 CPU 吗 ?

是的

## 当 user program 在运行的时候, OS 此时占有 CPU 吗 (单 processor) ?

不占有

## 什么操作会发起一个 syscall ?

比如: 打开一个文件, 发送消息给其他机器, 创建一个新的进程...

## 当 OS 与 user program 之间使用 *合作式运行*, user program 什么时候会将 CPU 给 OS ?

应用产生不合法的行为, 比如除以 0, 访问不可访问的内存, 都会产生 trap into OS.

## 如果 user program 是一个恶意程序, 使用 loop 一直消耗 CPU 的资源而不释放, 怎么办 ?

此时我们引入 timer interrupt 的概念

## 什么是 timer interrupt ?

用于每隔一定时间产生一个 interrupt,

当 interrupt 产生时, 当前正在运行的 process 会停止,

然后 OS 中一个预先配置好的 interrupt handler 会运行,

此时, OS 重新获得了 CPU 的控制权.

当系统启动:

1 OS 需要通知 hardware, trap table 的位置

2 OS 启动 timer

## timer 是在 hardware 上维护的吗 ?

是的

*todo*

## 究竟是 OS 存储当前正在运行的 process 的 register ? 还是 hardware 存储 ?

OS 是发起方, hardware 是执行方.

内存处在 hardware, 真正存储的地方是 hardware,

OS 发起了存储指令给 hardware.

## 发生 process context switch, 需要进行什么工作 ?

OS 需要保持当前正在执行的 process 的 register 到对应 process 的 kernel stack 中,

以及将接下来将运行的 process 的 register 从它的 kernel stack 中恢复出来,

然后 OS 执行 return-from-trap 指令, process 运行.

[![image4.png](https://i.postimg.cc/sx71k4Dd/image4.png)](https://postimg.cc/PvftZZw2)

上图中有两种类型的 register save/restore:

1. 当 timer interrupt 发生, 当前运行的进程的 user register 会隐式地, 由 hardware 存储到对应 process 的 kernel stack 中

2. 当 OS 决定切换进程, kernel register 会显式地, 由 software (OS) 存储到 process 的内存中

## 如何切换不同的 process 的 kernel stack ?

改变 stack pointer, 指向另一个 process 的 kernel stack.

## stack pointer 维护在哪里 ?

维护在 hardware 内存中呗.

---

# 关于 OS 调度

## 可以将 process 称为 job ?

是的

## 介绍各种调度算法之前, 我们需要进行假设:

1. 每个 job 运行相同的时间长度

2. 所有 job 的 arrival time 一样

3. 一旦 job 开始, 会一直运行到结束

4. 所有 job 只会使用 CPU, 也就是没有 IO

5. 每个job 的运行时间是 scheduler 算法已知的

## 用什么去衡量 scheduler 算法的优劣 ?

衡量 scheduler 算法的优劣, 主要是衡量 性能.

可以使用

turnaround time: 一个job, 完成的时间 - job到达系统的时间

公式: `T(turnaround)  = T(completion) - T(arrival)`

## 有哪些 scheduler 算法 ?

First In, First Out (FIFO)

优化 turnaround time:

- Shortest Job First (SJF)

- Shortest Time-to-Completion First (STCF)

优化 Response time:

- Round Robin (RR)

## scheduler 算法: First In, First Out (FIFO) ?

这是最基础的算法, 先来的任务先执行

[![image5.png](https://i.postimg.cc/QdknqF5f/image5.png)](https://postimg.cc/FYzZrsTS)

A B C 三个任务 arrival time 相同, 于是 `T(arrival) = 0`

假设 A 先运行, 然后 B 运行, 然后 C 运行

假设它们的运行时间都是 10s

`average turnaround time = (10 + 20 + 30) / 3 = 20`

缺点:

如果我们忽略假设中的一个条件: *1 每个 job 运行相同的时间长度*

如果先到达的 A 运行时间太长

[![image6.png](https://i.postimg.cc/CKcPW9cD/image6.png)](https://postimg.cc/KKgDTpSv)

`average turnaround time = (100 + 110 + 120) / 3 = 110`

## scheduler 算法: Shortest Job First (SJF) ?

在同一时间点上, 先运行时间最短的任务, 然后是下一个运行时间最短的任务...

[![image7.png](https://i.postimg.cc/j5CgKv11/image7.png)](https://postimg.cc/Fd5gVbNb)

`average turnaround time = (10 + 20 + 120) / 3 = 50`

缺点:

如果我们忽略假设中的一个条件: *2 所有 job 的 arrival time 一样*

假设 B, C 在 10s 才到

[![image8.png](https://i.postimg.cc/rmbhpxXw/image8.png)](https://postimg.cc/476vLKxD)

`average turnaround time = (100 + (110-10) + (120-10)) / 3 = 103.33`

## scheduler 算法: Shortest Time-to-Completion First (STCF)

在该 scheduler 算法中, 我们忽略假设中的一个条件: *3 一旦 job 开始, 会一直运行到结束*

[![image9.png](https://i.postimg.cc/D0Xx40nd/image9.png)](https://postimg.cc/gLGVfYwn)

当一个 job 进入系统, STCF scheduler 算法会在所有 job 中, 挑选运行时间最短的 job 先运行

`average turnaround time = ( (120-0) + (20-10) + (30-10) ) / 3 = 50`

## 抢占式 / 非抢占式 有什么不同 ?

抢占式会停止现在正在运行的 job, 运行另一个新选中的 job

SJF 是 非抢占式 的调度算法

STCF 是 抢占式 的调度算法

## Response Time 是什么 ?

这是引入的一个新的衡量 scheduler 算法性能的标准

Response Time 主要适用于有用户发生交互的情况下, 如何提高交互的性能. 更快的响应用户.

Response Time 计算为: job首次运行的时间 - job到达的时间

`T(response) = T(firstrun) - T(arrival)`

eg:

A 在 0s 到达, 运行 10s, 而 B, C 在 10s 到达, 运行 10s

分别的 `T(response)` 为 0, 0(B 到达了就马上运行), 10s

## scheduler 算法: Round Robin (RR) ?

该算法不是将 job 一直运行到结束, 而是运行一个时间片 (time slice), 然后切换到另一个在运行队列中的 job 运行.

time slice 的长度需要是 timer-interrupt 时间段 的整数倍

eg:

当不使用 RR, 只是用 SJF

[![image10.png](https://i.postimg.cc/TwZZ4ZP0/image10.png)](https://postimg.cc/N9kJyCm2)

`T(response) = (0 + 5 + 10) / 3 = 5`

如果使用 RR, time slice 为 1s:

[![image11.png](https://i.postimg.cc/7YrFM8dY/image11.png)](https://postimg.cc/JtKYMgR9)

`T(response) = (0 + 1 + 2) / 3 = 1`

可以看出, time slice 越短, T(response) 越小.

但是在 RR 算法下,

`average turnaround time = ( 13 + 14 + 15 ) / 3 = 14`

十分糟糕的 turnaround time.....

的确, RR 做法其实是拉长了每个 job 的运行时间

## time slice 的长度为什么需要是 timer-interrupt 时间段 的整数倍 ?

在运行了 time slice 的时间长度以后, 刚好发生 timer interrupt, 可以进行 job 切换

## time slice 太短会造成什么问题 ?

会造成 context switch 过于频繁, 带来的性能消耗会变大.

## context switch 会产生什么性能问题 ?

当发生 context switch, 不仅需要 OS 保存和恢复 register.

当 process 运行, 会建立很多状态, 包括 CPU cache, TLBs, 很多预测器...

切换 process, 这些状态性的东西都会被刷新, 新的状态重新载入.

## 如果 process 发起 IO请求之后, scheduler 不切换其他 process ?

接下来我们会忽略假设的一个条件: *4 所有 job 只会使用 CPU, 也就是没有 IO*

当一个 job 发起 IO 请求, job 会一直被 blocked 住, 等待 IO 完成

当 IO 完成了, 会发起一个 interrupt, OS 运行, 并将原来的 process 置为 ready 状态

## 如果 process 发起 IO 请求之后, scheduler 不切换其他 process, 会发生怎样的结果 ?

假设 A 运行 50ms CPU, 但是每运行 10ms 会发起一个 运行 10ms 的IO.

[![image12.png](https://i.postimg.cc/J439pYC3/image12.png)](https://postimg.cc/fJL2LKLk)

## 假设我们使用 STCF 调度算法 ?

实际上, A 被拆分成了 5个 10ms 的子任务, B 是一个 50ms 的任务.

当第一个 A 子任务完成, 只剩下 B 任务, 会运行 10ms 的 B 任务, 然后 IO完成, A子任务 ready, STCF 选择第二个 A 子任务运行....

[![image13.png](https://i.postimg.cc/8z5tLZpz/image13.png)](https://postimg.cc/qzPsTX6P)

## 什么情况下, SJF 与 FIFO 的 turnaround time 一样 ?

同时来的 job, 先到的 job length <= 后到的 job length

## 什么情况下, SJF 与 RR 的 Response time 一样 ?

SJF 同时来 3 个 job length 为 1, 1, 1 (slice time 为 1)

## 当 SJF 的 job length 增加, Response time 会有什么趋势 ?

递增

## 当 RR 的 slice time 增加, Response time 会有什么趋势 ?

递增

## 为什么要引入 MLFQ, MLFQ 想要解决什么问题 ?

MLFQ: The Multi-Level Feedback Queue

这是一种 scheduler 算法

MLFQ 有很多不同的 queue, 每一个 queue 给予不同的优先级.

高优先级的队列会先运行

在同一优先级队列中的 job, 使用 RR 调度算法.

因此, MLFQ 的基础规则是:

1. 如果 优先级 A > 优先级 B, A 运行

2. 如果 优先级 A = 优先级 B, A & B 运行, 使用 RR 算法

为了解决问题:

1. 为了使用 SJF 或者 STCF, 需要知道 job 运行的时间, 但是实际上并不知道

2. 为了减小 Response time

## MLFQ 是如何给每个 job 确定其优先级的 ?

基于已观察到的历史的行为, 进行确定.

eg:

如果一个 job 等待键盘输入, 经常放弃 CPU, 会获得高优先级.

如果一个 job 长时间占领着 CPU, 会被 MLFQ 降低优先级.

## MLFQ 队列是怎么样的 ?

eg:

[![image14.png](https://i.postimg.cc/zv7Fgg4h/image14.png)](https://postimg.cc/jnWfVLxq)

以上 job 的优先级并不是固定的, 会随着时间被调整.

## MLFQ 如何调整优先级 ? 具体规则是什么 ?

MLFQ 在基础规则的基础上, 引入了接下来的规则:

3 当一个 job 进入系统, 会被置于最高优先级队列

4a 如果一个 job 运行消耗光了整一个 time slice, job 的优先级会被降低

4b 如果一个 job 运行, 还没消耗光整一个 time slice, 就已经放弃了 CPU, 会被保留相同的优先级

## 举例说明 MLFQ 如何调整优先级 ?

eg1:

当有一个长时间占据 CPU 的 job

[![image15.png](https://i.postimg.cc/XYpkrXLZ/image15.png)](https://postimg.cc/QFZTP8Rs)

time slice 是 10ms, 因为 job 一直占据整个 time slice, 所以该 job 的优先级一直被降低

eg2:

在 eg1 的基础上, 来了一个运行时间很短的 job

[![image16.png](https://i.postimg.cc/CxGLHsm9/image16.png)](https://postimg.cc/qN7TpyfG)

这种情况下 MLFQ 与 SJF 的算法有点类似.

B(grey) 在 100ms 的时候进入系统, B需要花费 20ms 完成 job.

B 在到达最低优先级队列之前, 完成了 job, 使用了 2 个 time slice

然后 A 继续运行.

MLFQ 并不知道进入系统的 job 的运行时间, 所以一开始默认进入系统的 job 是 short job, 放在最高优先级, 随着时间进行, 如果真的是 short job, job 会很快运行完毕, 如果是 long-job, 会被降低优先级.

eg3:

在 4b 规则中, 如果 job 还没运行完整个 time slice 就放弃 CPU, job 会一直保持一样的优先级.

比如来了一个交互性的 job B, 只会占用 CPU 1ms, 然后就发起 IO, MLFQ 会一直保持 B 在最高优先级.

[![image17.png](https://i.postimg.cc/VLMbpMKW/image17.png)](https://postimg.cc/Yhr9m4Kv)

## MLFQ 存在什么问题 ?

如果存在太多交互性的 job (也就是占据 CPU 很短的 job), 那么那些占据 CPU 比较长的 job 就永远得不到运行.

有可能出现作弊行为, 比如:

某个 program 在运行到 99% time slice 的时候, 发起 IO, 放弃 CPU, 那么这个 job 就会一直在最高优先级队列.

可能另一种可能, 一个 program 一开始是 CPU-bound job (也就是比较长时间使用 CPU), 不过一段时间之后, 转换为 交互性 job, 但是此时 job 已经丢失了优先级.

## 如何解决 CPU-bound job 可能一直得不到运行的问题 ?

加入一个新的规则:

*5 运行了一段时间 S 以后, 将所有的 job 都移动到最高优先级队列*

可以解决两个问题:

1 job 不会被遗忘, 即使是 CPU-bound job, 每次 boot 都运行一段时间

2 当 job 从 CPU-bound 转化为交互性 job, 后来会保留其高优先级

[![image18.png](https://i.postimg.cc/v8CV0Xtm/image18.png)](https://postimg.cc/K4rjzPVX)

上图左侧中, 因为没有优先级 boot 机制, 低优先级队列一直得不到运行.

## 如何解决作弊行为 (达到 99% 的时候, 放弃 CPU, 从而将自己保留在高优先级队列中) ?

我们重写规则 4a 和 4b

*4 如果 job 消耗完了在对应队列中分配给它的时间 (无论放弃了多少次 CPU), 它的优先级会降低.*

[![image19.png](https://i.postimg.cc/0Nt6DCNP/image19.png)](https://postimg.cc/WDkbPgrH)

如上图右侧.

## MLFQ 还存在哪些问题 ?

eg:

需要多少队列 ?

time slice 的大小 ?

priority boot 的时间间隔是多少 ?

不同的队列会有不同的 time slice, 高优先级的队列的 time slice 较短. 低优先级的队列较长.

[![image20.png](https://i.postimg.cc/Wz0bWmsD/image20.png)](https://postimg.cc/H8sgjycH)

## 可以人为的提高或者降低 job 的优先级吗 ?

使用 nice 命令

## 综上所述, MLFQ, 为什么叫 MLFQ ?

有 multiple levels 队列,

使用 feedback 机制决定 job 的优先级.

## 总结 MLFQ 的 5 个规则 ?

1. 如果 优先级A > 优先级B, A 运行

2. 如果 优先级 A = 优先级 B, A & B 运行, 使用 RR 算法

3. 当一个 job 进入系统, 会被置于最高优先级队列

4. 如果 job 消耗完了在对应队列中分配给它的时间 (无论放弃了多少次 CPU), 它的优先级会降低.

5. 运行了一段时间 S 以后, 将所有的 job 都移动到最高优先级队列

## 什么是 seed ?

一个 random seed (seed state / seed) 是一个数字 (或者vector), 被用来初始化 伪随机数生成器.

## 什么是 Proportional Share ?

一种调度算法,

**不考虑** 优化 turnaround time 和 response time,

而 **考虑** 保证每一个 job, 获取到确定的百分比的 CPU 时间

## 什么是 lottery scheduling ? 有什么优势 ?

使用了 tickets 的机制

比如总共 100 tickets, job A 持有 75 tickets, job B 持有 25. 则 A 获得 75% 的 CPU 时间, 而 B 25%

lottery scheduling 的优势是 使用了 randomness.

1 random 可以避免边缘情况

2 random 是轻量级的, 每个 process 只需要记录很少的 state (eg: process tickets)

3 random fast.

## ticket *currency* 机制是什么 ?

允许每个用户定义自己的 tickets 数量, 每个 user 自己内部可以定义不同的 tickets 换算关系.

eg:

user A 和 user B, 每个 user 各有 100 tickets,

A 运行 2 job, B 运行 1 job

A 分别给 2个 job 分配 500 tickets, 换算为全局 tickets 为 50 tickets

[![image23.png](https://i.postimg.cc/vBcrQzr4/image23.png)](https://postimg.cc/tZGZkFmb)

## ticket *transfer* 机制是什么 ?

ticket 传递, 一个 process 暂时地将自己的 tickets 切换给另一个 process.

大多数用在 client / server 中, client 发送一个请求给 server 想让 server 执行, client 将自己的 tickets 都传递给 server, 最大化 server 的性能. 结束以后, tickets 回归到 client

## ticket *inflation* 机制是什么 ?

一个 process 可以暂时地提高或者降低自己的 tickets 的数量

ticket inflation 会应用在一组 process 之间互相信任的环境下

比如 一个 process 需要 CPU, 可以使用 ticket inflation 增加自己的 tickets

## lottery scheduling 如何实现 ?

[![image24.png](https://i.postimg.cc/sXqp1hmw/image24.png)](https://postimg.cc/TLjy4hZD)

[![image25.png](https://i.postimg.cc/GtDvDmSr/image25.png)](https://postimg.cc/GTcBwdCS)

上面, 总共有 400tickets, 假如取得一个随机数 300, 遍历该 list, 使 counter = 0 递增到 >300, 遍历到的节点就是 winner.

为了使这种实现的效率更高, 一般会从高到低排序每个 job. 这样可以遍历更少的 number

## lottery scheduling 的难点是什么 ?

如何合理的分配 tickets

## 如何判断 lottery scheduling 的合理性 ?

引入 U(unfairness metric)

如果分配了相同的 tickets, job 运行相同的时间, 那么两个 jobs 应该在同一时间点附近运行完成.

假设有两个 job, 每个 job 分配相同的 tickets(100), 每个 job 运行相同的时间(R)

U 定义为 第一job完成的时间 除以 第二job完成的时间

如果 jobs 几乎在相同的时间完成, U 会接近 1

当两个 jobs, 运行时间从 1 -> 1000, 下图是他们的 U

[![1.png](https://i.postimg.cc/dVwG7vRP/1.png)](https://postimg.cc/0bXz3THc)

当 job 运行时间足够长, lottery scheduling 就符合预期.

## stride scheduling 是什么 ?

在 tickets 的机制上, 建立一种 deterministic fair-share scheduler

job 拥有的 stride 与 tickets 的数量相反

比如 job A, B, C 分别拥有 100, 50, 250 tickets, 使用 10000 作为被除数,

A, B, C process 的 stride 分别为 100, 200, 40

当每一次 process 运行, 用 pass += stride 来跟踪 process 的全局进度.

scheduler 使用 stride 和 pass 决定接下来运行哪一个 process.

基础思想是, 在任意给定时间, 选择一个 pass 值最低的 process 运行.

eg:

[![2.png](https://i.postimg.cc/zDhh030Y/2.png)](https://postimg.cc/6T9TQWkM)

## 举例说明 stride scheduling ?

[![3.png](https://i.postimg.cc/mrhzMrtq/3.png)](https://postimg.cc/mcfgWB3N)

## 既然 stride scheduling 可以保证 job proportional share 运行, 为什么还需要 lottery scheduling ?

考虑一种情况, 当 stride scheduling 运行一段时间以后, 系统进入一个新的 job, 那么这个 job 的 pass 应该是多少 ? 如果是 0, 则该 job 会一直垄断 CPU, 显然不合适.

lottery scheduling 的好处是: no global state per process. 没有一直维护一个全局的进程状态

## Completely Fair Scheduler(CFS) 是什么 ?

用于实现 fair-share scheduling, 优势是 高效率 和 可扩展性

大多数 scheduler 是基于固定 time slice

CFS 将 CPU 平均的分配给所有竞争的 process.

基于一个技术: virtual runtime (vruntime)

























