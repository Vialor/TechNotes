# TCP header
**data payload**: TCP header, data
**TCP header**:
![[Untitled 8.png|Untitled 8.png]]
src port, dst port: independent port from ports of UDP (TCP port 80 is HTTP, UDP port 80 is undefined)  
sequence number: to make sure the order of data  
each byte has a sequence number, and sequence number in TCP header is for the first byte of the segment  
acknowledgement: receiver sends back Ack to inform the sender that segment has been received, otherwise sender resend packet  
Ack = the sequence number of the last byte in the received segment + 1  
flags: SYN, FIN, RESET, PUSH, URG, ACK  
SYN: establish connection; FIN: close connection  
ACK: acknowledgement field is valid, should pay attention to it  
URG: contain urgent data; UrgPtr: point to the last urgent data byte in this segment (bytes after are nonurgent)  
# TCP connection
## Connection establishment
Three-way handshake algorithm 三报文握手
Client ⇒ Server: SYN, Sequence number = x
Server ⇒ Client: SYN, Sequence number = y, ACK, Acknowledgement = x+1
Client ⇒ Server: ACK, Acknowledgement = y+1

|            | client: server can receive my message? | client:  I can receive server's message? | server: client can receive my message? | server: I can receive client's message? |
| ---------- | -------------------------------------- | ---------------------------------------- | -------------------------------------- | --------------------------------------- |
| handshake1 | Unknown                                | Unknown                                  | Unknown                                | Yes                                     |
| handshake2 | Yes                                    | Yes                                      | Unknown                                | Yes                                     |
| handshake3 | Yes                                    | Yes                                      | Yes                                    | Yes                                     |
### Decide MSS during SYN
**Maximum Sending Segment MSS**
MSS wants to be as large as possible but not more than: MTU − TCP/IP header size(usually 40B) because it wants to avoid **IP fragmentation**.
IP fragmentation could cause network overhead and the risk of losing packets.

The sender suggests an MSS value in its initial TCP packet, and the receiver can either accept it or propose a different MSS value in response. For the remainder of the TCP connection, the sender uses the suggested MSS from receiver, and the receiver uses the suggested MSS from the sender.
## Data transfer
example:
Client ⇒ Server: Sequence number = 1, Ack = 1, Length = 100 bytes
Server ⇒ Client: Sequence number = 1, Ack = 101, Length = 200 bytes
Client ⇒ Server: Sequence number = 101, Ack = 201, Length = 50 bytes
Server ⇒ Client: Sequence number = 201, Ack = 151, Length = 500 bytes
## Connection termination
Four-way handshake algorithm 四报文挥手
1. Client ⇒ Server: I want to terminate connection
2. Server ⇒ Client: I have received your termination request
3. Server ⇒ Client: I have done transferring data to you
4. Client ⇒ Server: Ok, we shut down the conversation
# Flow Control & Congestion Control
**BDP bandwidth delay product** = bandwidth * delay
**Flow control**: avoid overloading the receiver
	sender receives RWS  from receiver to make sure that SWS ≤ RWS
**Congestion control**: avoid overloading the network
	sender calculates cwnd size to make sure that SWS ≤ cwnd size
## TCP Flow Control: Sliding Window  
**发送窗口 Sending window**
LAR last acknowledgement received: SeqNo of the last Ack received
LFS last frame sent: SeqNo of the last frame sent
SWS sending window size: upper bound on the un-Acked frames to be sent
LFS - LAR ≤ SWS

**接收窗口rwnd Receiving window**：接收方根据目前接收缓存大小所设定的窗口值
LFR: last frame received: SeqNo of the last frame received
LAF: largest acceptable frame: SeqNo of the largest acceptable frame
RWS receiving window size
LAF - LFR ≤ RWS

**Silly Window Syndrome** happens when the sending/receiving window is silly small, and thus causes significant overhead and ineffective use of the bandwidth.  
**Nagle’s Algorithm**: a defense against senders that cause silly window syndrome: the sender sends the first segment even if it is a small one, then it waits until an ACK is received or a MSS is accumulated for future sends.
## Congestion Control
**拥塞窗口cwnd congestion window**：发送方根据自己估算的网络拥塞程度设置的窗口值，反映了网络的当前容量。
- **慢启动阶段 Slow Start**：拥塞窗口指数级增长，直到**慢启动阈值ssthresh**
- **拥塞避免阶段 Congestion Avoidance**：拥塞窗口线性增长，直到达到接收窗口大小。如果没有收到应当到达的TCP确认报文段而产生了超时重传，拥塞窗口就减小为发送方cwnd的一半，然后重新进入慢启动阶段，使得拥塞路由器有时间将队列中积压的分组处理掉。
- **快速重传 & 快速恢复 Fast Retransmit & Fast Recovery**：当接收方检测到丢包时，会发送重复的 ACK（确认报文）。当发送方收到三个重复的 ACK 时，它会判断某个数据包丢失，并立即重新传输丢失的数据包，而不需要等待超时。此时，TCP 不会直接进入慢启动，而是将拥塞窗口减半，进入拥塞避免阶段。