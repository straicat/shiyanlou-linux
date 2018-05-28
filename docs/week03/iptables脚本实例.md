# iptables脚本实例

默认规则：

``` Bash
# 查看当前生效的规则
$ sudo iptables-save
# 以:开头的规则为默认规则，默认所有chain都是ACCEPT
```

清除规则：

``` Bash
# 清除规则链中已有的条目
iptables -F
# 清除用户自定义的空链
iptables -X
# 清空链和链中默认规则的计数器
iptables -Z
```

载入模块：

``` Bash
modprobe ip_tables # 启动 iptables
modprobe iptable_nat # 使用 nat 表时载入
modprobe ip_nat_ftp # 在出去的包做了伪装以后，就必须加载该模块，否则防火墙无法知道返回的包该转发到哪里
modprobe ip_conntrack_ftp # 使防火墙能够识别 FTP 某类特殊的返回包
modprobe ip_conntrack # 使防火墙具有连接跟踪能力
modprobe ipt_MASQUERADE # 数据包伪装的作用
# 查看加载的ip相关模块
$ lsmod | grep ip
```

打开lo回环：

``` Bash
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
```

设置默认策略：

``` Bash
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD DROP
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

根据需求制定各项规则：

``` Bash
# 允许服务器ping对方主机而不允许对方主机ping服务器
iptables -A INPUT -p icmp -–icmp-type 8 -j DROP
iptables -A OUTPUT -p icmp --icmp-type 8 -j ACCEPT
iptables -A INPUT -p icmp --icmp-type 0 -j ACCEPT
# 开放主机对dns的访问
iptables -A INPUT -p udp --sport 53 -j ACCEPT
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
# 开放 SSH 的端口，实现远程连接
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# FTP 服务器，开启 21 端口
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
iptables -A INPUT -p tcp --dport 10000:20000 -j ACCEPT
# WEB 服务器,开启 80 端口
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```
