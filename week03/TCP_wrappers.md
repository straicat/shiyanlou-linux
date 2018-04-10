# TCP wrappers

TCP wrappers 主要是通过对系统中的某些服务进行开放和关闭、允许和禁止来有效保证系统的安全运行。

服务需链接到`libwrap`才支持TCP wrappers过滤。

``` Bash
# 查看服务是否链接到libwrap
$ ldd /path/to/service | grep libwrap
# 查看sshd是否链接到libwrap
$ ldd /usr/sbin/sshd | grep libwrap
```

TCP wrappers 主要是依靠`/etc/hosts.allow`（优先级更高）和`/etc/hosts.deny`来实现防火墙的功能，规则立即生效。

规则基本格式：

```
服务名:IP、IP段或主机名:action
```

|通配符|含义|
|---|---|
|`ALL`|所有|
|`LOCAL`|名字里不带`.`的主机|
|`KNOWN`|已知主机|
|`UNKNOWN`|未知主机|
|`PARANOID`|主机名和主机地址不匹配的主机|
|`.demo.com`|`demo.com`域名内所有主机|
|`192.168.`|同IP网段的所有主机（`192.168.*.*`）|
|`*`|代替0或多个字符|
|`IP地址/子网掩码`|控制某个网段的地址的访问|

action：

- `spwan` 开启一个shell执行指定的命令
- `twist` 替换访问者的请求为指定的命令

|参数|含义|
|---|---|
|`%a`|客户端的 IP 地址|
|`%A`|服务端的 IP 地址|
|`%d`|守护进程的名字|
|`%h`|客户端的主机名|
|`%H`|服务端的主机名|
|`%p`|守护进程的 pid|
|`%u`|客户端的用户名|

【配置示例】

- 只允许192.168.0.0/16子网和本地访问Xvnc
- 其他IP地址访问Xvnc时，会记录访问信息到日志/var/log/vncdeny.log，并拒绝访问
```
# /etc/hosts.allow
Xvnc: 192.168.0.0/16， LOCAL
# /etc/hosts.deny
Xvnc: ALL : spawn (/bin/echo %a from %h attempted to access %d >> /var/log/vncdeny.log) &
```

---

【参考】

- `man hosts_access`
- `man hosts_options`
