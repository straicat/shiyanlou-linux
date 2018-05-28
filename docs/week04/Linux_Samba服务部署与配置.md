samba配置文件：`/etc/samba/smb.conf`，具体配置参数可参考：`/etc/samba/smb.conf.example`

可以通过`testparm`命令测试samba服务的配置情况

`pdbedit`命令可以管理samba用户数据库

- `-L` 列出当前用户数据列表
- `-v` 详细模式
- `-u` 用户名
- `-a` 创建用户
- `-x` 删除用户

需要关闭SELinux，否则无法看到共享目录的内容。

