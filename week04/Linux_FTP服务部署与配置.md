FTP是明码传输，安全性低。FT有两种模式：主动模式、被动模式。

安装FTP客户端和服务器端：

``` Bash
$ sudo yum install ftp
$ sudo yum install vsftpd
```

编辑配置文件`/etc/vsftpd/vsftpd.conf`：

``` ini
# 监听ipv4
listen=YES
# 监听ipv6
# listen_ipv6=YES

# 允许匿名用户登录
anonymous_enable=YES
# 匿名用户不检验密码
no_anon_password=YES
# 添加登录成功后的欢迎提示，banner file的内容为欢迎提示内容
banner_file=/path/to/banner/file
# 匿名用户只能下载文件，也就是只读
anon_world_readable_only=YES
# 设置写入权限
write_enable=YES
# 匿名用户上传权限
anon_upload_enable=NO
# 匿名用户创建目录权限
anon_mkdir_write_enable=NO
# 匿名用户其它写入权限（如删除、重命名）
anon_other_write_enable=NO
# 支持ASCII模式的下载（传输纯文本文件时，将换行符换成目的操作系统的换行符）
ascii_download_enable=YES

# 匿名用户最大数据传输率（单位：B/s）
anon_max_rate=10000000
# 最大同时上线人数
max_clients=100
# 同IP最大连接数
max_per_ip=3
# 连接响应超时时间
data_connection_timeout=30
# 一段时间没响应就断线（单位：s）
idle_session_timeout=300

# 允许访客登录
guest_enable=YES
# 访客用户用户名（默认：ftp）
guest_username=ftp_vuser
# 用户主目录可写
allow_writeable_chroot=YES
# 允许本地用户登录
local_enable=YES
# 访客虚拟用户配置文件目录
user_config_dir=/path/to/guest/users/config/dir
# 指定pam认证文件（目录：/etc/pam.d/）
pam_service_name=vsftpd

# 允许本地用户登录
local_enable=YES
# 本地用户创建文件的默认umask权限
local_umask=022
# 限制本地用户只能访问家目录
chroot_local_user=YES
# 家目录可写
allow_writeable_chroot=YES
# chroot_local_user=YES时，chroot_list_file里的用户可以访问家目录以上的目录
# chroot_local_user=NO 时，chroot_list_file里的用户不可以访问呢家目录以上的目录
chroot_list_enable=YES
chroot_list_file=/path/to/chroot/list/file
# 用户访问控制，可以设置/etc/vsftpd/user_list里的用户能否登录
userlist_enable=YES
userlist_deny=YES
```

vsftpd同时监听ipv4和ipv6：

``` Bash
# 使用两个配置文件
$ sudo cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd_ipv6.conf
# 两个配置文件分别设置listen=YES和listen_ipv6=YES
# 启动服务
$ sudo systemctl start vsftpd.target
```

FTP命令：

``` Bash
# 下载单个文件
ftp> get test.txt
# 多文件下载
ftp> mget *.txt
# 下载时不提示确认（在下载前执行）
ftp> prompt off
# 上传文件
ftp> put upload.txt
# 执行Shell命令（前面加!）
ftp> !ls
```

虚拟用户配置：

``` Bash

```
