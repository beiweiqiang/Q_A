
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

## 如何区分 应用(application) 和 应用层协议(application-layer) ?

protocol 是 application 的一部分, 比如 Web application 会包括:

- 标准文档格式 (HTML)
- Web browser
- Web server (既包含 client, 也包含 server)
- protocol (HTTP)

## HTTP 是什么 ?

HyperText Transfer Protocol

HTTP 通过 2 个 program 进行实现: client program 和 server program

HTTP 基于 TCP transport protocol

HTTP client 会先与 server 端初始化建立起一个 TCP 连接, 然后 browser process 与 server process 通过 socket 接口访问 TCP

client 发送 HTTP request message 到 socket 接口, 以及从 socket 接口中接收 HTTP response message

server 端发送 message 给 client 端, 并不会记录任何与 client 有关的信息 (HTTP 是 stateless protocol), 以致于下一次相同的 client 请求相同的 message, server 会重复一样的工作

对于 Web page 在 client 如何进行解析, HTTP 毫不关心

80 端口是 HTTP 的默认端口

*todo*

## 什么是 Web page ?

一个 Web page (或者称为 document) 包含很多的 objects, 一个 object 是一个 file, 比如 HTML file, JPEG file... file 通过 URL 进行指定

## URL 的组成 ?

每个 URL 由两部分组成, server 的 hostname 和 object 的 path name

## layer architecture 的优点 ?

协议被设计成分层结构, 优点, eg:

HTTP 不需要担心 数据的丢失以及 TCP 如何从 loss 中恢复数据, 这是 TCP 以及 protocol stack 底层 protocol 的工作

## 为什么会有两种 connection 方式: non-persistent connection 和 persistent connection ?

一系列的请求可能是连续性的到来, 可能是周期性的到来, 也可能是不定期的到来.

client-server 的交互是建立在 TCP 上的, 需要决定:

- 每个 request/response 通过分开的 TCP connection 进行 ?
- 所有的 request/response 在同一个 TCP connection 进行 ?

HTTP client 是默认 persistent connection 的, 但是可以配置为 non-persistent connection

## non-persistent connection HTTP 是如何进行的 ?

1 HTTP client process 与 server 初始化 TCP connection

... (建立 TCP connection)

server 在发送完 response message 以后, 会*主动通知 TCP 关闭 TCP connection*, 但是 TCP 实际上不会马上关闭 connection, 会等到 client 接收到 message 以后才进行 terminate connection

## 什么是 RTT ?

round-trip time (RTT), 一个 small packet 从 client 发送到 server, 然后再从 server 返回到 client 经历的时间.

RTT 包括:
1. packet processing delay
2. packet propagation delay
3. packet queuing delay

## 从 TCP connection 开始建立, 到 file 完成传输共需要多久 ?

[![10.png](https://i.postimg.cc/g2CJ03M5/10.png)](https://postimg.cc/TKcG75vq)

client 发送一个 small TCP segment 给 server, server 答复并响应一个 small TCP segment, 最后 client 答复 server.

three-way handshake 中的首两次花费了一个 RTT

接着 client 发送 HTTP request message 以及 three-way handshake 的第三步, 可以一同发送到 TCP connection

因为 server 发送 file 需要 transmission delay

整个关于 file 的 request/response 时间是 2 个 RTT 加上 server 的 transmission delay time

## non-persistent connection 有什么缺点 ?

1. 对于每个 connection, 在 client 端和 server 端都需要分配 TCP buffer 和 TCP variables, 对于 server 端更严重, 因为会同时服务多个 client
2. 对于每个 file object, 都需要经历 2 个 RTT (1 个 RTT 建立 TCP connection, 1 个 RTT request/response object)

## persistent connection 什么时候关闭 TCP 连接 ?

HTTP server (HTTP client 也可以发起) 会在 TCP connection 不使用一段时间以后进行关闭, 该时间可以配置

## 有多少种 HTTP message ?

有 2 种类型的 HTTP message: request message, response message

## HTTP request message 格式是怎样的 ?

eg:
```
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/5.0
Accept-language: Fr
```

message 使用 ordinary ASCII text 编写

每一行会跟着一个 carriage return 和一个 line feed, 最后一行会有额外的, carriage return 和 line feed, 即 CRLF

第一行称为 request line, 接下来的行称为 header lines

request line 包含 3 个 field: method field, URL field, HTTP version field

method field 可以取的值: GET, POST, HEAD, PUT, DELETE

Host 行表示请求的 object 位于哪里, 在 Web proxy caches 中 host header line 是必须的, 其他情况下非必须

Accept-Language header是 content negotiation headers 之一

[![11.png](https://i.postimg.cc/Y0D2BFGd/11.png)](https://postimg.cc/ctYGQrc3)

在 request message 的最后部分是 entity body

form表单请求不一定都要使用 POST method, 也可以使用 GET method

HEAD 与 GET 类似, 但是 HEAD 返回的 message 中不会包含 request object

## HTTP response 的格式是怎样的 ?

eg:
```
HTTP/1.1 200 OK
Connection: close
Date: Tue, 09 Aug 2011 15:44:04 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 09 Aug 2011 15:11:03 GMT
Content-Length: 6821
Content-Type: text/html

(data data data data data ...)
```

response message 包括 3 部分: status line, header lines, entity body

status line 包含 3 个 fields: protocol version field, status code, status message

下面解析每个 header 的作用:
- `Connection: close` 告诉 client 发送完这个 message 以后会关闭 TCP connection
- `Date: ` 指示 server 创建 HTTP response 的时间. 不是 object 创建或者上一次修改的时间, 而是 server 从 file system 中获取 object, 将 object 插入 response message, 并发送 response message 的时间.
- `Server: ` 类似于 client 的 `User-agent: `
- `Last-Modified: ` 指示 object 创建或者上一次修改的时间, 在 object caching 中作用很大

object 的类型是通过 `Content-Type: ` 指示, 而不是通过文件后缀类型

[![12.png](https://i.postimg.cc/KvdF7xn3/12.png)](https://postimg.cc/ppYgRNcP)

## telnet 工具使用 ?

比如 `telnet cis.poly.edu 80` 会打开一个与 host 关联的 TCP 连接, 然后可以进行 HTTP request:

```
GET /~ross/ HTTP/1.1
Host: cis.poly.edu
```

## 用户如何通过 Cookies 与 server 交互 ?

因为 HTTP server 是 stateless, 为了对用户保持跟踪, 引入了 Cookies.

cookie 技术由 4 部分组成:
1. 在 HTTP response message 包含 cookie header line
2. 在 HTTP request message 包含 cookie header line
3. 用户的 end system 中会维护并管理一份 cookie file
4. web site 会有自己的 database

server 发送 `Set-cookie: ` 到 client, client 将指定的 cookie添加到 cookie file, 接下来对 site 的每次 request 都会带上该 cookie

之后, 如果用户在这个 site 上进行了注册, server 会将该用户与过去的 cookie 行为进行绑定

Cookies 就好像在 stateless HTTP 上创建了一个 user session layer

## LAN 是什么 ?

*todo*

## Web cache 是什么 ?

又称为 proxy server

行为类似于原始 Web server, 可满足 HTTP request

Web cache 有自己的硬盘存储, 会将最近 request 的 object 拷贝一份存储下来

[![13.png](https://i.postimg.cc/7Yc7gGs1/13.png)](https://postimg.cc/svYvzXR1)

步骤:
1. browser 与 Web cache 建立 TCP connection, 然后向 Web cache 发送一个 HTTP request 请求 object
2. Web cache 检查自己是否有对应 object 的拷贝, 如果有, Web cache 直接向 client 发送 HTTP response
3. 如果 Web cache 没有对应的 object, Web cache 会与 origin server 建立 TCP connection, 然后 Web cache 向 origin server 发送 HTTP request 请求对应的 object
4. Web cache 接收到来自 origin server 的 object 以后, 保存一份 copy 在自己本地, 然后再向 client 发送 HTTP response (client 与 Web server 的 TCP connection 已经建立过了)

可以看出, Web cache 既是 server 也是 client

优点:
1. 减少对于 origin server 的 client request
2. 减少机构 access link 到 Internet 上的流量, 减少在 Internet 中的流量

## 在 Web cache 中, hit rates 指标是什么 ?

request 可以通过 cache 进行满足的概率, 一般是 0.2 到 0.7

## 什么是 Conditional GET ?

如果 file 一直缓存在 Web cache 会引发另一个问题 file 过期了怎么办 ?

因此, HTTP 有一个机制, 允许 web cache 校验该 object 是否是最新的, 这种机制叫做 Conditional GET

一个 HTTP request message 是 Conditional GET 需要满足:
1. request message 是 GET method
2. request message 包含 `If-Modified-Since: ` header

从 origin server 返回给 Web cache 的 response header 里面会包含 `Last-Modified: ` header, web cache 会将其存下代表该文件的最后修改时间

当 Web cache 需要校验这个 object 的时候, 会在 request 中带上 `If-Modified-Since: ` 并且该值与之前的 server 返回的 `Last-Modified: ` 值一致, 用于校验该 object 在这个时间点之后是否有更新过, 如果更新过, origin server 需要返回最新的 object, 如果这个时间点之后 object 没有更新, origin server 会返回 304 Not Modified, 此时 response 中的 data field 为空

## File Transfer: FTP 如何建立交互 ?

用户首先提供远程 host 的 hostname, 本地的 FTP client process 与 FTP server 建立 TCP 连接.

用户提供用户标识和密码, 作为 FTP command 通过 TCP 连接发送给 FTP

## HTTP 和 FTP 有什么不同 和 相同 ?

相同:
1. 都是 file transfer protocol
2. 都建立在 TCP 之上

不同:
FTP 使用了并行的两个 TCP 连接:
1. 一个是 control connection, 用于在两个 host 之间发送 control 信息, 比如用户标识, 密码, 一些操作 remote host 的指令
2. 一个是 data connection, 用于用户文件的传输

[![14.png](https://i.postimg.cc/yYVgchyS/14.png)](https://postimg.cc/3WcxHv6K)

因此我们说:
- FTP 的 control information 是 out-of-band
- HTTP 的 control information 是 in-band (在相同的 TCP 连接里既传输 control information 也传输 file)

## FTP 如何进行 data transfer ?

在一个 FTP session 中, control connection 会一直持续, 但是每传输一个 file 都会创建一个新的 data connection

在一个 session 内, FTP server 需要记住该 user 的 state, 比如该 user 正在浏览哪个目录

## FTP 的 commands 和 replies ?

为了分隔开连续的 commands, 使用 carriage return 和 line feed 分隔每个 command

每个 command 包含 4 个大写的 ASCII 字符, 以及可选的参数, 比如:
- USER username: 发送 user 标识给 server
- PASS password: 发送 user password 给 server
- LIST: 让 server 传送回当前目录下的文件列表 (server 会新建一个 data connection)
- RETR filename: 从 remote host 获取当前目录下一个 file (server 会新建一个 data connection)
- STOR filename: 用于将一个 file 发送给 remote host

每个 command 都会返回一个 reply. reply 格式是一个 status code 加上一个 phrase, 比如:

331 Username OK, password required

## Internet mail system 包括哪些组件 ?

包括三部分: user agents, mail servers, Simple Mail Transfer Protocol(SMTP)

[![15.png](https://i.postimg.cc/6qzQYSf4/15.png)](https://postimg.cc/vxgMm0jG)

user agents 让用户可以进行 read, reply, forward, save, compose message

当用户 compose 完 message 进行发送, message 会发送到他的 mail server, message 进入 mail server 的 outgoing message queue, 待发送队列

当用户想要阅读 message, user agent 会从 mail server 中拉取最新的 message 回来

SMTP 是 Internet electronic mail 的 application-layer protocol

SMTP 有两端: client side 和 server side, 两端都是 mail server, 双方角色可以互换, 发送方作为 client side

## 如何标识一个 host ?

使用 hostname, 但是 hostname 包含不同长度的字母符号, 对应 router 来说处理会困难

host 也通过 IP address 进行标识

## IP address ?

一个 IP address 包含 4 个字节, 有一个固定的层级结构

比如 121.7.106.83

每一个区间是十进制数 0~255

## DNS 主要提供了什么服务 ?

人们通常更习惯于记忆 hostname 标识, 但是 routers 更倾向于长度固定的, 具有层级结构的 IP 地址, 为了协调, 我们需要一个 directory service 提供 *将 hostnames 翻译为 IP 地址的服务*.

这是 DNS (domain name system) 的主要服务

## DNS 由什么组成 ?

DNS 是一个由层级结构的 DNS servers 实现的 distributed database

DNS 是一个 application-layer protocol, 允许 host 查询 distributed database

DNS protocol 运行在 UDP protocol 之上, 使用 port 53

DNS 通常会被其他 application-layer protocol 使用, 比如 HTTP, SMTP 和 FTP, 用于将用户提供的 hostname 翻译为 IP 地址

## DNS 的翻译服务是如何工作的 ?

1. 用户机器运行 DNS client
2. browser 从用户输入的 URL 中提取 hostname, 发送给 DNS client
3. DNS client 发送请求给 DNS server
4. DNS client 收到 reply, 包含 hostname 的 IP 地址
5. browser 从 DNS client 接收到 IP 地址, 开始初始化 TCP 连接

因为加多了一个 hostname 翻译的过程, DNS 会添加额外的 delay, 不过, IP 地址通常会被缓存在 "最近" 的 DNS server 中

## DNS 还提供什么其他服务 ?

1 Host aliasing

一个复杂的 hostname 可以有 1 个或多个更方便人类记忆的 alias name

原始 hostname 称为 canonical hostname, 别名 hostname 称为 alias hostname

2 Mail server aliasing

比如提供给用户的 mail 地址是 bob@hotmail.com, 但是 Hotmail mail server 的 hostname 实际上可能会复杂

3 Load distribution

在多个相同的 server, 但是 IP 地址不同, 多个 IP 地址可以关联到一个 canonical hostname

DNS 对外进行翻译的时候, 对应同一个 canonical hostname, 提供不同的 IP 地址让 client 进行选择

## 如果 DNS 系统不做成分布式的, 而是集中式 (centralized design) 的, 有什么缺点 ?

1 a single point of failure

如果这个 DNS server crash, 整个 Internet 都 crash

2 traffic volume

一个 DNS server 需要处理所有 DNS query

3 distant centralized database

一个 DNS server 不能靠近所有的 querying client, 因此该 server 离某些 client 会非常遥远

4 maintenance

该 DNS server 需要维护所有 Internet 中的 host, 且经常需要进行更新

所以 DNS 会引入 distributed 技术, DNS 是 Internet 中实现 distributed database 的一个极好的例子

不是一个 DNS server 有所有的 hsot mapping. 这些 mapping 会分布在所有的 DNS server 中.

## DNS server 分为多少层 ?

大概会分为三层 DNS server:
1. root DNS server
2. top-level domain (TLD) DNS server (com, org, edu...)
3. authoritative DNS server (yahoo.com, amazon.com...)

[![16.png](https://i.postimg.cc/KYQscDfr/16.png)](https://postimg.cc/FfdxDc27)

实际上还有一类重要的 DNS server: local DNS server

严格上来说 local DNS server 不属于层级之一, 但却是 DNS architecture 组成之一

每个 ISP(Internet Service Provider) 都有一个 local DNS server

当一个 host 发送一个 DNS query, query 会发送到 local DNS server, local DNS server 扮演着 proxy 的角色, 将该 query 转发给 DNS server 中的其他层级

## local DNS server 如何工作 ?

[![17.png](https://i.postimg.cc/cLWSrTRV/17.png)](https://postimg.cc/N5dVn6P4)

代替 host 进行 host 中的 DNS client 进行工作

1. host 中 DNS client 将 DNS query message 发送给 local DNS server
2. local DNS server 将 query 转发给 root DNS server, root DNS server 返回其中一个 edu TLD DNS server IP 地址
3. local DNS server 将 query 发送给 TLD DNS server, TLD DNS server 返回 `umass.edu` authoritative DNS server IP 地址
4. ...

注意: 之前我们都假设 TLD DNS server 知道 authoritative DNS server 的 IP 地址, 但是不总是知道的, 可能 TLD DNS server 只知道一个 intermediate DNS server, 这种情况下 local DNS server 还需要再经历一次 intermediate DNS server query

## 有多少种 DNS query 的方式 ?

两种: recursive queries (迭代查询) 和 iterative queries (重复查询)

之前上图是 iterative queries

下图是 recursive queries:

[![18.png](https://i.postimg.cc/Y9WnNBk8/18.png)](https://postimg.cc/r0qNcbDt)

## DNS client 如何与 DNS server 沟通 ?

比如我们访问 www.amazon.com

1. client 会先与其中一个 root server 沟通, 获取 `com` TLD server 的 IP 地址
2. client 与其中一个 TLD server 沟通, 获取 `amazon.com` authoritative server 的 IP 地址
3. client 与 amazon.com 其中一个 authoritative server 沟通, 获取 `www.amazon.cn` 的 IP 地址

## DNS caching 有什么作用 ?

1. 提高 DNS query 的 delay performance
2. 减少 DNS message 在 Internet 中的流转数量

在一个 query chain 中, 当 DNS server 接收到一个 DNS reply, 会将接收到的 hostname-IP mapping 保留在本地内存

比如 local DNS server 会保留各级 DNS server 查询结果. 通常会保留 2 天然后才 discard

## resource records (RRs) 是什么 ?

DNS 分布式数据库存储的 数据记录

## RR 的格式是怎样的 ?

resource record 是一个 four-tuple (四个元素的 item): (Name, Value, Type, TTL)

TTL 是记录 resource record 的时间, 用于记录 cache 什么时候需要移除, 下面我们先忽略 TTL

## RR 的 Type 有什么类型 ?

有四种 Type:
- A

提供标准的 hostname 到 IP address 的映射, 比如: (relay1.bar.foo.com, 145.37.93.126, A)

- NS

是 domain 与 authoritative DNS server 的映射, 比如: (foo.com, dns.foo.com, NS)

用于找到 authoritative DNS server 以后再次进行链式查询

- CNAME

记录 alias hostname 与 canonical hostname 的映射, 比如: (foo.com, relay1.bar.foo.com, CNAME)

- MX

记录 alias hostname 与 canonical mail server 的映射, 比如: (foo.com, mail.bar.foo.com, MX)

## DNS query message 和 DNS reply message 的格式是怎样的 ?

[![22.png](https://i.postimg.cc/dVGM8fmq/22.png)](https://postimg.cc/zyDPN0cM)

DNS query message 和 DNS reply message 的格式相同

分为几部分:

- header section

开头的 12 bytes 是 header section, 其中 Identification 是 message 的唯一标识, 对应的 query 与 reply 的 Identification 一致

在 Flags 区域, 有 1-bit 用于区分 query/reply flag

- question section

包含用于 query 的 message, 比如 name, type

- answer section

包含 DNS server 返回的 resource record, 会包含 Type, Value, TTL

- authority section

- additional section

## 一个 network application 包括什么 ?

经典的 network application 包含 2 个 program, 一个 client program 和一个 server program, 运行在两个不同的 end system 中

## 有几种类型的 network application ?

1. 实现了开源协议, 比如 RFC 的 network application

如果 client 和 server 实现 RFC 协议, 就要使用熟知的端口

2. 实现的协议是私有的

避免使用熟知的开源协议的端口

*todo 如何实现私有的协议 ?*

## 开发一个 network application 首先需要考虑什么 ?

application 运行在 TCP 还是 UDP 之上

## 如何基于 UDP 创建一个 socket program ?

每个 process 类似一个 house, 每个 process 的 socket 类似一个 door

application 的 developer 可以控制 application-layer 侧 socket 的任何东西, 但是对于 transport-layer 侧的 socket 控制甚少

process 在将 packet 发送给 socket 之前, 需要给 packet 依附上目标地址和目标 port, 当一个 socket 创建以后, 会分配给他一个 port number, 作为标识符

发送方的 source address 和 port 也会被依附到 packet 中, 但是这不是在 application-layer 做, 而是 OS 完成的.

[![19.png](https://i.postimg.cc/52zJsy1W/19.png)](https://postimg.cc/75qj6xfK)

## 如何基于 TCP 创建一个 socket program ?

TCP application, 在 client 和 server 开始发送数据之前, 需要进行 TCP connection 的建立

建立了 TCP 连接以后, 只需要将 data 丢进 socket 就行, 不需要指定 destination address, 与 UDP 不一样, UDP 在每次将 data 丢进 socket 之前都需要给 packet 添加 destination address

基于 TCP 的 socket program, 会为每个 client 建立一个新的 socket, 这是与特定 client 之间的 connection socket, 而 server 运行时建立的 socket 只是用于接收 TCP handshake 的第一部分的 client knock, 之后 server 会为每个 client 建立单独的 connection socket (对于 client 端, 只有一个端口, 对于 server 端, 有两个端口, 如下图)

[![20.png](https://i.postimg.cc/SKph34N2/20.png)](https://postimg.cc/jDvmnGft)

[![21.png](https://i.postimg.cc/JhcLWFGT/21.png)](https://postimg.cc/bGJMx30b)

## browser 请求 image url 是 serial 还是 parallel ?

parallel

如下图的 1, 2 行

[![23.png](https://i.postimg.cc/sfB3Mw82/23.png)](https://postimg.cc/0rqT3d9g)

## http client 除了 browser 还有什么 ?

*todo*

## 如何进行 HTTP access authentication ?

已经建立了一套 HTTP Access Authentication Framework

HTTP client 如果想要访问的 page 属于 protected realm

server 会返回 401 Unauthorized 状态码和 WWW-Authenticate header field

在该 header 里会说明 client 需要如何进行相应的 authentication

client 进行必要的认证, 再次发送 request, 并带上 Authentication header, header 里包含 client 的 credentials

如果 server 接受该 credentials, 则返回对应的 request page

如果 server 不接受则依旧返回 401

在 WWW-Authenticate header 和 Authentication header 中包含的内容依赖于选择的 authentication scheme

## 有哪些 authentication scheme ?

目前广泛使用的是两种 scheme:

Basic Access Authentication

Digest Access Authentication

## Basic Access Authentication 工作原理 ?

server 返回 401 并带上 WWW-Authentication header, header 中包含 token "Basic" 和一个 name-value pair, value 指定 protected realm 的 name, eg:

`WWW-Authenticate: Basic realm="Control Panel"`

对应的, client 端会要求获取 username 和 password

client 在下一次 request 中会带上 Authentication header, header 的内容是带上 token "Basic" 和一个 base64-encoded 字符串, 字符串值为 base64(username:password), eg:

`Authorization: Basic QWRtaW46Zm9vYmFy`

缺点: 网络监听者 获取得到 Authorization header 以后就可以通过 base64-decode 得到 username 和 password

## Digest Access Authentication 工作原理 ?

引入了密码学, 通常使用 MD5 加密

MD5 会将一个任意长度的字符串计算为 128 位的数字, 同时MD5 是一个 one-way function

但是仍然存在一个问题, 如果监听者拿到了 md5 hash string, 并使用该 string 对 server 进行重复发送, 模拟了与用户相同的请求, 这称为 replay attack

为了解决 replay attack, 因此引入了 nonce 机制,

server 返回 401, 在 WWW-Authenticate header 中, 会包含 token "Digest", realm="XXX", nonce="XXX" 和其他必要字段, 其中 nonce 是一个唯一的, 由 server 维护的值

client 接收到以后, 结合 username, password, realm, nonce 进行加密计算, 再发送给 server,

因为 server 不会接收两次一样的 nonce, 所以可以避免 replay attack

一个简单的计算算法:

```
HA1 = MD5(username:realm:password)
HA2 = MD5(method:digestURI)
response = MD5(HA1:nonce:HA2)
```




