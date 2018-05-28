# TCP/IP简介

查看本机网关：`ip route show`

查看最大传输单元（MTU）：`netstat -i`

OSI七层网络模型：物理层，数据链路层，网络层，传输层，会话层，表示层，应用层。

TCP/IP四层模型：网络接口层，网络层，传输层，应用层。

TCP报文：

![TCP报文首部]()

标志位：

- URG：紧急指针有效
- ACK：确认序号
- PSH：尽快交付
- RST：重置连接
- SYN：同步序号
- FIN：释放连接

UDP报文首部只有8个字节，包括：源端口、目的端口、长度、校验。

TCP三次握手：

TCP四次挥手：

超时重传：

`ip`命令：

- `link` 网络设备
- `address` 设备的协议地址
- `route` 路由表条目
- `neighbour` ARP/NDISC缓冲区条目

理论较多，详见《计算机网络》
