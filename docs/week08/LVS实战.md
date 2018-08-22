## LVS实战

集群系统中，服务器资源可分为负载均衡调度器(Load Balancer)和后端服务器群。

## LVS/NAT

Load Balancer需要进行的操作：

1、安装`ipvsadm`

2、开启内核路由转发

```Bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

3、配置ipvs规则，定义集群服务：

```Bash
# 添加一个新的集群服务
sudo ipvsadm -A -t <Load Balancer的对外IP>:<端口号> -s <调度算法>
# 添加一个集群规则
sudo ipvsadm -a -t <Load Balancer的对外IP>:<端口号> -r <后端服务器内网IP> -m
```

选项说明：

- `-A` 添加一个新的集群服务
- `-t` 使用TCP协议
- `-s` 指定负载均衡调度算法，LVS实现了8种调度算法，如`rr`（轮询算法）等。
- `-a` 添加一个集群规则
- `-r` 指定后端服务器内网IP
- `-m` 定义为NAT

## LVS/DR

后端服务器需要进行的操作：

1、修改内核参数：

```Bash
echo 1 > /proc/sys/net/ipv4/conf/lo/arp_ignore
echo 1 > /proc/sys/net/ipv4/conf/all/arp_ignore

echo 2 > /proc/sys/net/ipv4/conf/lo/arp_announce
echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce

# 使上述配置立即生效
sysctl -p
```

`arp_ignore`参数定义了本机响应ARP请求的级别，含义：

- `0` （默认）目标IP为本机则响应ARP请求
- `1` 接收ARP请求的网卡IP和目标IP相同，则响应ARP请求

`arp_announce`参数定义了发送ARP请求时，源IP应该填什么，含义：

- `0` （默认）使用任一网络接口上配置的本地IP地址
- `2` 忽略IP数据包的源IP地址，总是选择网络接口所配置的最合适的IP地址作为ARP请求数据包的源IP地址

2、配置网卡别名：

```Bash
# 配置虚拟IP
ifconfig lo:0 192.168.0.10 broadcast 192.168.0.10 netmask 255.255.255.255 up

# 添加路由，因为本就是相同的网段所以可以不添加该路由
route add -host 192.168.0.10 dev lo:0

# 重新加载网卡
service networking restart
```

Load Balancer需要进行的操作：

1、安装`ipvsadm`

2、创建eth0的别名并绑定对外IP，作为集群同时使用：

```Bash
ifconfig eth0:0 192.168.0.10 netmask 255.255.255.0 up
```

3、配置ipvs规则：

```Bash
# 添加一个新的集群服务
sudo ipvsadm -A -t <Load Balancer的对外IP>:<端口号> -s <调度算法>
# 添加一个集群规则
sudo ipvsadm -a -t <Load Balancer的对外IP>:<端口号> -r <后端服务器内网IP> -g
```

`-g` 定义为DR模式
