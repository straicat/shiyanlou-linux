# Linux磁盘管理

`uname` 查看系统信息：

``` Bash
$ uname
Linux
$ uname -a
Linux 50963fc4948a 3.13.0-125-generic #174-Ubuntu SMP Mon Jul 10 18:51:24 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
$ uname -m
x86_64
# 打印网络节点的主机名
$ uname -n
50963fc4948a
$ uname -o
GNU/Linux
$ uname -s
Linux
# 打印内核版本
$ uname -r
3.13.0-125-generic
```

`lspci` 查看硬件信息（需要先安装`pciutils`）

`lsmod` 显示已载入系统的模块的信息

`df` 查看磁盘空间情况

`sda1`含义：s（SATA）d（硬盘,disk），a表示第一块硬盘，1是分区号

可以使用`fdisk`进行磁盘分区，`mkfs.ext4`将分区格式化为ext4文件系统，`mount`挂载


