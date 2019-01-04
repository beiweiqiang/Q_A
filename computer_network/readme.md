
# 关于计算机网络

## 什么是 Internet ?

可以从 2 个方面来描述 Internet:

1. 基本的硬件和软件组成了 Internet

2. Internet 为分布式应用提供网络服务

## 什么是 end system ? host ?

处于互联网边缘的网络设备, 比如我们的 PCs, Macs, ...

也被称为 host, 因为其上运行着应用程序

host 有时候会被分为两类: clients 和 servers

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

## 什么是 protocols ?

用于控制 end system, packet switches 或者 Internet 中的其他组件, 其在 Internet 中发送和接收信息

[![image22.png](https://i.postimg.cc/sDSJQwtS/image22.png)](https://postimg.cc/zVzh4SDX)

A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

协议 定义了在两个或更多个通信实体之间交换的消息的格式和顺序，以及对传输和/或接收消息或其他事件所采取的动作。

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

