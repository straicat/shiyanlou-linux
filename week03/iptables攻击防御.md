# iptables攻击防御

DDOS攻击方式：

- Ping of flood
- Ping of Death
- Teardrop attacks
- UDP flood
- SYN flood
- CC

SYN攻击原理：通过发送大量的半连接请求，耗费服务器CPU和内存资源。

TCP三次握手：

```
         Client                         Server
            |                              | LISTEN
            | SYN=1, seq=x                 |
   SYN_SENT |----------------------------->|
            |                              |
            | SYN=1, ACK=1, seq=y, ack=x+1 |
            |<-----------------------------| SYN_RCVD
            |                              |
            | ACK=1, seq=x+1, ack=y+1      |
ESTABLISHED |----------------------------->| ESTABLISHED
            |                              |
```

SYN flood：服务器变SYN_RCVD状态后客户端不予响应，服务器会一直等待直至超时，这种连接被称为半连接。大量这种半连接称为SYN flood。

SYN攻击模拟：

``` Bash
# 使用ab工具模拟高并发
# -n 请求数  -c 并发量
$ ab -n 10000000 -c 600 http://localhost/
# 查看SYN连接数
$ netstat -an | grep SYN
# 增加iptables进行防范
# 对SYN限制美妙最多1个连接，接收第三个数据包时触发
$ sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
# 或者设置每个客户端并发数
$ sudo iptables -A INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 20 -j REJECT
```

还可以通过修改TCP的配置（`/etc/sysctl.conf`）来进行防范。

CC攻击：模拟多个用户不停访问需要大量数据操作的页面，造成网络堵塞，属于TCP全连接攻击。可以使用域名欺骗解析、更改Web端口、IIS屏蔽IP等方式进行防御。
