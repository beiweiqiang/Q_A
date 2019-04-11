
# Computer Network

## TCP 连接 3 次握手, 为什么是 3 次而不是 2 次 ?

TCP 是一个双向的通信协议, 为了保证每一方的数据都可以可靠传输, 每一方都需要与对方建立通信.

双方通过 sequence number 来跟踪发送的数据顺序,

sequence number 并不是从 0 开始的, 而是一个初始随机数,

双方都需要发送 seqnum, 并收到对方的确认.

eg:
[![27.png](https://i.postimg.cc/kg671NK4/27.png)](https://postimg.cc/NKthLHNq)

上图, 实际上, 我们需要 4 次通信:
1. A --> B: A 发送随机序列号 X, 想与 B 建立通信
2. A <-- B: B 接收到了序列号 X, 回复应答序列号 [X+1]
3. A <-- B: B 发送随机序列号 Y, 想与 A 建立通信
4. A --> B: A 接收到了序列号 Y, 回复应答序列号 [Y+1]

以上, 实际上, 中间的两个 event 会发生在同一个 packet 中.

所以步骤简化为:

[![28.png](https://i.postimg.cc/rFbkrd3L/28.png)](https://postimg.cc/7fgdvLqB)

以上, 注意 SYN 和 ACK, 都是双向的.

## sequence number 为什么不是从 0 开始 ?

为了防止 sequence prediction attacks


