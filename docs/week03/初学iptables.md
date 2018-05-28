# 初学iptables

Linux防火墙：Netfilter/iptables

- Netfilter：工作在内核空间，执行过滤
- iptables：工作在用户空间，编写过滤规则

语法格式：

```
iptables [-t table] COMMAND chain CRETIRIA -j target
-------------------------------------------------------------------------
|          | table  | COMMAND |   chain     |   CRETIRIA   |  target    |
|          |--------|---------|-------------|--------------|------------|
|          | filter |    -A   | PREROUTING  | -p tcp       | ACCEPT     |
|          | nat    |    -I   | FORWARD     |    udp       | DROP       |
|          |        |    -R   | INPUT       | -s           | REJECT     |
| iptables |        |    -D   | OUTPUT      | -d           | SNAT       |
|          |        |    -P   | POSTROUTING | --sport      | DNAT       |
|          |        |    -L   |             | --dport      | MASQUERADE |
|          |        |         |             | -m limit     | REDIRECT   |
|          |        |         |             |    connlimit |            |
|          |        |         |             |    state     |            |
-------------------------------------------------------------------------
``` 

表：

|table|说明|支持的链|
|---|---|---|
|`filter`|默认的表，包含过滤规则|INPUT、FORWARD、OUTPUT||
|`NAT`|包含源、目的地址和端口转换使用的规则|PREROUTING、OUTPUT、POSTROUTING|
|`mangle`|设置特殊的数据包路由标志的规则|五个链都可以|
|`raw`|实现数据跟踪，是新增的表|PREROUTING、OUTPUT|

优先级：filter < NAT < mangle < raw

|COMMAND|描述|
|---|---|
|`-A`|新增规则，在当前链的最后新增一个规则|
|`-I`|插入规则，把当前规则插入为第几条|
|`-R`|替换/修改第几条规则|
|`-D`|删除第几条规则|
|`-P`|设置默认策略|
|`-L`|显示指定表和指定链的规则|

``` Bash
# 清除规则链中已有的条目
$ sudo iptables -F
# 清除用户自定义的空链
$ sudo iptables -X
# 清空链和链中默认规则的计数器
$ sudo iptables -Z
```

链：

|chain|描述|
|---|---|
|PREROUTING|用于路由判断前的规则，如修改目的地址（DNAT）|
|INPUT|数据包通过路由计算判断为本地的 Linux 系统则通过此链的检查|
|OUTPUT|用来针对所有本地生成的包|
|FORWARD|两个网络连接时，用于传递数据包，两个网络间的数据包必须流经该防火墙|
|POSTROUTING|用于路由判断后的规则，如修改源地址（SNAT）|

target：

|target|描述|
|---|---|
|`ACCEPT`|满足匹配条件就接受数据包|
|`DROP`|满足匹配条件就丢弃数据包|
|`REJECT`|和 DROP 相似，拦截数据包，并返回错误信息|
|`SNAT`|源网络地址转换|
|`DNAT`|目的网络地址转换|
|`MASQUERADE`|和 SNAT 的作用相同，区别在于它不需要指定`–to-source`|
|`REDIRECT`|满足匹配条件就将转发数据包到另一个端口|

NAT类型：

|名称|全称|IP地址对应|
|---|---|---|
|静态NAT|static NAT|一对一|
|动态NAT|dynamic NAT或pooled NAT|多对多|
|NAPT|Network Address Port Translation|多对一|

```
      IN                              OUT
       |                               ^
       v                               |
   PREROUTING                     POSTROUTING
       |                              ^  ^
       v                              |  |
Routing Decision ---> FORWARD --------/  |
       |                                 |
       v                                 |
     INPUT  --------> localhost -----> OUTPUT
```

数据包状态：

- NEW ：数据包开始一个新连接（重新连接或将连接重定向）
- RELATED ：数据包是基于某个已经建立的连接而建立的新连接。
- ESTABLISHED ：只要发送并接到应答，一个数据连接就从 NEW 变为 ESTABLISHED ，而且该状态会继续匹配这个连接的后续数据包。
- INVALID ：数据包不能被识别属于哪个连接或没有任何状态的，比如内存溢出，收到不知属于哪个连接的 ICMP 错误信息，一般会 DROP 这个状态的任何数据。

``` Bash
# 查看当前iptables的规则
# -t 查看指定的table（默认filter）
# -n 不解析IP和port
# -L 列举iptables的规则
# --line 显示规则编号
$ sudo iptables [-t table] -nL --line

# 查看已经生效的规则
# -t 指定table（默认全部）
# 可以重定向以便恢复
$ sudo iptables-save [-t table]
# 恢复规则
$ sudo iptables-restore < xxx.txt
```
