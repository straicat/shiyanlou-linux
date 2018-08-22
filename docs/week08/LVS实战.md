## LVS实战

集群系统中，服务器资源可分为负载均衡调度器(Load Balancer)和后端服务器群。

## LVS/NAT实现

Load Balancer需要进行的操作：

1、安装`ipvsadm`

2、开启内核路由转发

```Bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

3、配置ipvs规则，定义集群服务：

```Bash
sudo ipvsadm -A -t <Load Balancer的外网IP>:<端口号> -s <调度算法>  # 添加一个新的集群服务
sudo ipvsadm -a -t <Load Balancer的外网IP>:<端口号> -r <后端服务器内网IP> -m  # 添加一个集群规则
```

选项说明：

- `-A` 添加一个新的集群服务
- `-t` 使用TCP协议
- `-s` 指定负载均衡调度算法，LVS实现了8种调度算法，如`rr`（轮询算法）等。
- `-a` 添加一个集群规则
- `-r` 指定后端服务器内网IP
- `-m` 定义为NAT

