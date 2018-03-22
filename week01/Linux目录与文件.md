文件类型：

* `-` 普通文件
* `d` 目录
* `c` 字符设备文件
* `b` 块设备文件
* `s` 本地域套接口
* `p` 管道
* `l` 符号链接

权限编码：

|数字|字符|
|---|---|
|4|r|
|2|w|
|1|x|

`chmod`改变文件或目录的权限。参数中，`u`代表所有者，`g`代表所属组，`o`代表其他用户，`a`代表所有人。

`chown`改变归属关系：

* `chown 所属者 文件`
* `chown 所属者:所属组 文件`
* `-R` 递归应用权限

`chgrp`改变归属组

`tac`与`cat`正好相反，为倒序显示

显示前3行：`head -3 xxx`

显示末尾3行：`tail -3 xxx`

`stat`显示文件的状态

创建软链接：`ln -s 源文件 链接文件`

ACL权限：

* `mount -o remount,acl /` 重新挂载根分区，并加入ACL权限
* `dumpe2fs` 获取ext2, ext3或ext4文件系统的信息
* `getfacl` 获取指定文件的ACL权限
* `setfacl` 设置指定文件的ACL权限
    * `sudo setfacl -m u:用户:rwx 文件` 给用户设定ACL权限


