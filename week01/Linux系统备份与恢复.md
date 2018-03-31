# Linux系统备份与恢复

`dump` 备份：

``` Bash
# -0 备份等级（0为完整备份，1为增量备份）
# -f 指定备份文件
$ sudo dump -0 -f /home/test.dump /
```

`restore` 恢复：

``` Bash
# -C 显示备份文件和当前文件系统的区别
# -f 指定备份文件 -D 指定要比较的文件系统
$ sudo restore -C -f /home/test.dump -D /
# -r 还原整个文件系统
# -T 指定需要恢复的路径
$ sudo restore -r -f /home/test.dump -T /home
```

-----

使用`tar`进行备份：

``` Bash
# 备份/etc目录
# -g 指定快照文件，快照文件记录文件更改，从而实现增量备份
$ sudo tar -cvf backup.tar -g metadata /etc
```

使用tar备份时，会自动删除路径开头的`/`以在恢复时不与根目录冲突

-----

`dd`命令：

``` Bash
# 使用dd命令将/etc/hosts复制为当前目录下的hosts
# if=输入文件 of=输出文件
$ dd if=/etc/hosts of=hosts
# 创建1G大小的文件
# bs=块大小  count=块数目
# /dev/zero能源源不断产生二进制0
$ dd if=/dev/zero of=test bs=1M count=1024
```


