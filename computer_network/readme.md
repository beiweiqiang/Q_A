
# 关于计算机网络

## 什么是 Internet ?

可以从 2 个方面来描述 Internet:

1. 基本的硬件和软件组成了 Internet

2. Internet 为分布式应用提供网络服务

## 什么是 end system ? host ? 他们有什么不同 ?

处于互联网边缘的网络设备, 比如我们的 PCs, Macs, ...

也被称为 host, 因为其上运行着应用程序

host = end system, 二者没有本质上不同, 只是 host 有时候会被分为两类: clients 和 servers

## Web server 是一个 end system 吗 ?

是的

## 传输速率 (transmission rate) 使用什么作单位 ?

bits/second

## routers 和 link-layer switches ?

两种类型的交换机都将 packet 转发到最终目的地.

link-layer switches 使用在 access networks

routers 使用在 network core

## 什么是 packet switches ?

分组交换机

将一条 communication link 上到来的 packet, 转发到出口的另一条 communication link

## ISP 是什么 ?

Internet Service Providers: 互联网服务的提供商

end systems 通过 ISP 接入 Internet

[![image21.png](https://i.postimg.cc/fLwc6sLf/image21.png)](https://postimg.cc/Ty45DZnK)

## 什么是 packets ?

当一个 end system 想要给另一个 end system 发送数据, 这些数据信息会被拆分成一个个 segment, 然后为 segment 添加 header 信息, 因此数据信息就会变成一个个的 packet, 到达终点的 end system 以后, packet 进行重组, 变为原始数据

*todo*

## 什么是 protocols ? 为什么 protocol 那么重要 ?

用于控制 end system, packet switches 或者 Internet 中的其他组件, 其在 Internet 中发送和接收信息

[![image22.png](https://i.postimg.cc/sDSJQwtS/image22.png)](https://postimg.cc/zVzh4SDX)

A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

协议 定义了在两个或更多个通信实体之间交换的消息的格式和顺序，以及对传输和/或接收消息或其他事件所采取的动作.

在 network 中, 不同的 protocol 被用于完成不同的通信任务.

## 什么是 congestion-control ?

拥塞控制

*todo*

## TCP 和 IP 协议 ?

Transmission Control Protocol / Internet Protocol

IP protocol 指定了 routers 和 end systems 之间的的 packet 发送和接收的格式

*todo*

## 什么是 access network ?

物理上, 将 end system 连接到第一个 router 的 network

## DSL modem 与 cable modem 的作用 ?

DSL 使用已经存在的老旧的电话线, 在同一个电话线链路上, 发送数据信号, 数据信号的频率与电话信息的频率不同, 复用了电话设施, 使用了 DSL modem

我曹, 有线电视的链路也可以被复用, 使用了 cable modem

## 什么是 propagation delay ?

the time it takes for the bits to travel across the wire at near the speed of light.

*todo*

## Forwarding Tables ?

router 从上一条通信链路接收到 packet, 如何决定转发到哪一条通信链路 ?

packet 中会带上 destination 的 IP addresss

每个 router 有自己的 forwarding table, 上面会记录 destination address 和 outbound link 的 mapping

*todo*

## circuit switching 和 packet switching 有什么不同 ?

circuit switching: 在 end system 的会话一直持续的过程中, 在通信链路上所需要资源, 比如 buffer 会一直保留着. 因此其传输速率是不变的.

packet switching: 不会保留资源, 会按需使用资源, 所以会造成一种结果: 有时候需要等待访问通信链路. 其传输速率可能会发生变化.

## 有哪些类型的 delay ?

当 packet 经过传播路径上的每个 node, 都会经历各种类型的 delay, 比如:

nodal processing delay, queuing delay, transmission delay, propagation delay, 这些 delay 累加起来叫 total nodal delay

[![4.png](https://i.postimg.cc/3NYtpRC4/4.png)](https://postimg.cc/62bVxBPt)

nodal processing delay:

如上图, packet 从上游已经到达 router A, router A 会检查 packet 的 header 决定该 packet 需要传输到哪个通信链路

queuinng delay:

packet 等待被传输到通信链路, 等待前面的 packets 都被传输出去

transmission delay:

将 packet 从 router A 传输到 output link 的开头, 即 packet 的长度 / 传输速度

propagation delay:

当一个字节被传输到 link 上, 开始传播到 router B, 即 距离 / 传播速度

从 link 的开头传播到 router B 需要的时间称为 propagation delay, 该 delay 依赖于 link 的物理媒介

一旦 packet 的最后一个字节到达 router B, packet 存储在了 router B 中, 只有当 packet 中的所有字节全部到达 router B 了, router 才开始 processing

## 什么是 traffic intensity ?

假设 a 是 packet 平均到达 node queue 的速度 (单位是 packets / sec)

R 是 transmission rate (单位是 bits / sec)

L 是每个 packets 的长度

则我们定义 traffic intensity 为 La/R

当 La/R > 1, packet 到达 queue 速度超过了 packet 被传输出去的速度, 这种情况下 queue delay 会增长到无限长 !

我们要求 system 的 traffic intensity 不能超过 1, 即 La/R ≤ 1

## 为什么会产生 packet loss ?

因为在每个 router 中, queue 的容量是有限的, 如果一个 packet 到达的 router 中 queue 已经 full 了, router 会 drop 这个 packet

当 traffic intensity 越接近 1, lost 情况越严重

## network 中的分层结构是怎样的 ?

改变其中某一层的实现, 只要对外还是提供同样的服务, 对其他层没有任何影响, 下层为上层提供服务

每层的 protocol, 可能由 network 的 hardware 实现, 或者 software 实现, 或者二者一起实现

[![5.png](https://i.postimg.cc/0yw7kvDL/5.png)](https://postimg.cc/0z9znLtf)

Application-layer protocol 比如 HTTP, SMTP, 几乎都是由 software 实现在 end system

Transport-layer 与 Application-layer 类似, 也是几乎由 software 实现

physical-layer 和 data-link-layer 负责处理通信链路数据, protocol 由 hardware 实现

network-layer 通常由 hardware 和 software 一同实现

需要注意的是, 在 end system, packet switches, 以及组成 network 的其他组件之间, 也还有一层 protocol

不同层的 protocol 组合起来称为 protocol stack

## protocol stack 中各层的数据格式是怎样的 ?

- Application-layer

在 Application-layer 的数据形式是 message

- transport-layer

在 transport-layer 的数据形式是 segment, 是将 message 分隔为更小的 segment

Internet 有两个 transport protocol, TCP 和 UDP, 他们都可以 transport 应用层的 message, TCP 提供一个面向连接的服务给他的应用

- network-layer

network layer 中的数据形式是 datagrams (数据报)

network layer 有 IP 协议 和 许多的 routing 协议, Internet 是由网络组成的一个大网络, 在每个小网络的 network layer 中, network administrator 可以使用自己的 routing protocol, 但是都需要用 IP 协议, 因此 IP 是将网络组织在一起的胶水

- link-layer

link layer 的 packet 形式称为 frames

将 packet 从一个 node (router 或者 host) 传递到下一个 node, network layer 会依赖 link layer 的服务

在每一个 node 处, network layer 将 datagrams 向下传递给 link layer, link layer 将 datagram 沿着 router 递送到下一个 node, 下一个 node 的 link layer 将 datagram 向上传递给 network layer

该层的 protocol 包括比如 Ethernet, WiFi... 一个 datagram 可能会被不同的 link-layer protocol 进行处理

- physical-layer

physical layer 将 frames 中的单独字节从一个 node 传输到下一个 node, 该层的 protocol 主要决定于链路的 transmission medium 传输媒介

- 总结

message -> segment -> datagrams -> frames -> bits

## link layer 对于从 network layer 来的 datagrams 是原封不动的进行传递到下一个 node 吗 ?

*todo*

## 为什么需要 link layer ?

*todo*

## OSI 模型与 five-layer Internet protocol stack 有什么不同 ?

OSI 模型中添加了两层在 Application-layer 和 transport-layer 之间,

presentation-layer 和 session-layer

presentation-layer: 提供数据压缩和解压的服务

session-layer: 提供数据交换的 分隔 和 同步的服务, 包括构建 checkpointing 和 recovery scheme

这两层是否需要和是否重要, 取决于应用开发者

## 在 network 中, 数据如何流动 ?

[![6.png](https://i.postimg.cc/j2TR7zFH/6.png)](https://postimg.cc/G8M00srt)

上图中, routers 和 link-layer switches 都是 packet switches

不过他们没有实现所有的协议栈, 只实现了低层的一些协议

虽然 link-layer switches 无法识别 IP 地址, 但是它们可以识别 layer2 link-layer 的地址, 比如 Ethernet 地址

## transport-layer 给 message 加的 header 包括什么内容 ?

error-detection bits: 接收方用于检测 message 中是否有 bits 被改变

*todo*

## network-layer 给 segment 加的 header 包括什么内容 ?

比如 source 和 destination 的 end system 地址

##每一层的数据由什么组成 ?

在每个 layer, packet 有两个类型的 fields: header fields 和 payload field

payload 是从上一层来的 packet

## 什么是 DoS ?

denial-of-service

*todo*

## packet sniffer 的结构是怎样的 ?

[![7.png](https://i.postimg.cc/qBSjr4Fd/7.png)](https://postimg.cc/kDNxF39j)

packet capture library 将 link-layer frame 复制一份, 只需要 link-layer frame, 因为其中就包含了其上其他 layer 的数据信息

packet sniffer 由两部分组成: packet capture library 和 packet analyzer

packet analyzer 能理解 Ethernet frame 的格式, 可以从中区分出 IP datagram,

packet analyzer 能理解 IP datagram 的格式, 可以从中提取出 TCP segment,

packet analyzer 能理解 TCP segment 的结构, 可以从中提取出 HTTP message

packet analyzer 能理解 HTTP protocol, 就能知道 "GET", "POST"...

## wireshark 中的 protocol name ?

在 wireshark 中, 所有 protocol name 都是小写的

## wireshark 只是一个 packet analyzer 吗 ?

是的

## `ifconfig en0` 代表什么 ?

在 wireshark 中是, Wi-Fi en0

## 为什么处于 network-core 的 router 这些 component 不使用 5 layer 结构 ?

*todo*

## 有哪些 application architecture ?

有两种:

- client-server architecture
- peer-to-peer(P2P) architecture

## network architecture 与 application architecture 有什么不同 ?

network architecture 是指整个 network 的架构, 包含了处于 network-core 的 router 和 network-edge 的 end system

Application architecture 则是只有 end system 之间的通信

program 之间的通信实际上是 process 之间的通信, 我们可以说 process 是运行着的 program

在给予的通信会话的一对 process, 我们可以将 process 标签为 client 和 server

## 在如何定义通信会话中的 client 一方和 server 一方 ?

In the context of a communication session between a pair of processes, the process that initiates the communication (that is, initially contacts the other process at the beginning of the session) is labeled as the client. The process that waits to be contacted to begin the session is the server.

在一对进程之间的通信会话的上下文中, 启动通信的进程 (即, 会话最初发起联系的一方进程) 被称为 client, 在会话中等待被联系的 process 称为 server

## 什么是 socket ?

a software Interface, 有两个功能:

- 一个 process 可以将 message 发送给 socket
- process 从 socket 中接收 message

处于不同 end system 的两个 process 间使用 socket 进行通信

[![8.png](https://i.postimg.cc/7YtXjcHP/8.png)](https://postimg.cc/crYQnFtP)

socket 是 host 中, application layer 和 transport layer 之间的 interface

application developer 对于 application-layer 一侧的 socket 有足够的控制能力, 但是对于 transport-layer 一侧的 socket 则很少控制能力.

application developer 只能控制 transport-layer 侧 socket 的一部分内容:

1. transport protocol 的选择
2. 传递一些 transport-layer 的参数比如最大 buffer, 最大 segment sizes

## message 为了到达目的 process, 需要什么信息 ?

1. 目的 host 的地址: IP 地址
2. 接收的 process 的唯一标识符: port number

## transport-layer protocol 可以提供给 application 什么样的 service ?

如果 protocol 提供一个可靠的数据传递 service, 即 reliable data transfer, 此时发送方 process 只需将 data 发送给 socket, 就知道该 data 一定会无错误的到达接收方 process.

如果 transport-layer protocol 不提供可靠的数据传输, 称为 loss-tolerant application, 比如多媒体应用, 传统的 audio/video...

- reliable data transfer
比如: 银行存款

- throughput
比如: 网络电话对于 voice 的编码速率是 32kbs, 就需要保证这个吞吐速率

- timing: 保证 message 在一定时间内到达 receiver process
比如: 实时交互应用, 多人在线游戏...

- security
比如: sending host 的 transport protocol 对 data 进行加密, 在 receiving host 的 transport protocol 对 data 进行解密

*todo*

## throughput 是什么 ?

吞吐量

发送方 process 传递 bits 到接收方 process 的速率

## 一些 application 对于 network 的要求

[![9.png](https://i.postimg.cc/Y92WrrR5/9.png)](https://postimg.cc/xk7CgSW5)

## TCP 提供什么 service ?

TCP 提供一个面向连接的 service 和一个可靠的数据传输 service

- connection-oriented service:
TCP 在 application-level message 开始进行交互之前会需要先交换 client 和 server 的 transport-layer control information, 称为 handshaking. handshaking 阶段以后, 两个 process 的 socket 之间会建立起一个 TCP connection, 这个 connection 是全双工的, 即双发可以同时进行数据的发送, 当 application 结束 message 的发送, 必须结束这个 connection

- reliable data transfer service:
通信的process双方依赖 TCP 传输 data, 保证其传输无错误并保持合适的顺序.

- congestion-control
TCP 也提供 congestion-control 机制, 用于限制每个 TCP 连接的网络带宽

## TCP 如何提供 securing 服务 ?

*TCP 或者 UDP 没有提供加密的功能*, 即从 sending process 传入 socket 的数据, 与在 network 中, link 中传输的数据是一样的, 从而出现了 Secure Sockets Layer (SSL)

TCP-enhanced-with-SSL 提供了传统的 TCP 没有提供的 process-to-process 的安全性 service, 比如 encryption, data integrity, end-point authentication

SSL 并不是第三个 transport protocol, 而是 TCP 的一个 enhancement, 这个 enhancement *实现在了 application-layer*, SSL 在 application-layer 进行实现

如果 application 想要使用 SSL service, 需要在 client 侧和 server 侧都包含 SSL 代码.

SSL 有自己的 socket API 提供, 与传统的 TCP socket API 类似, 所以 application 此时是对 SSL socket 进行交互.

当使用了 SSL, sending process 将 cleartext data 传递给 SSL socket, 在发送方 process SSL 将数据进行进行加密, 然后再传递给 TCP socket. 加密了的数据到达接收方 socket, TCP socket 将数据传递给 SSL socket, 进行解密, 最后将 cleartext 通过 SSL socket 传递给 receiving process.

## UDP 的特点 ?

1. UDP 是轻量级的 transport protocol
2. UDP 是无连接的, 所以在两个 process 之间进行通信之前不会进行 handshake
3. UDP 提供不可靠的数据传输服务, 即当一个 process 发送 message 到 UDP socket, UDP 不保证 message 会被传输到 receiving process, 即使 message 到达 receiving process 了, message data order 也不能保证
4. UDP 不包含 congestion-control 机制

## application-layer protocol 定义 ?

application-layer protocol 定义了, 运行在不同 end system 上的 application process 之间如何传递 message

定义了:
- 交换的 message 的类型, 比如 request message 和 response message
- 不同 message 类型的语法
- 在一个 message 中, 不同字段的语义
- 一个 process 何时以及如何发送 message 或者响应 message

另外, 有些 application-layer 的协议是私有的, 而 HTTP 协议是开源的





