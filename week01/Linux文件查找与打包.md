## find

命令格式：

``` Bash
find 路径 参数
```

-----

根据文件大小查找：

`+n` 大于n；   `-n` 小于n；  `n` 等于n

b/c/k/w/M/G: block/字节/k/字符数/M/G  注意大小写！

``` Bash
# 查找/etc目录下文件大小大于100k的文件（注意k要小写）
$ sudo find /etc -size +100k
```

-----

根据时间戳查找：

* a->access
* c->change
* m->modify

`-atime`, `-ctime`, `-mtime` 根据访问时间，状态改变时间，内容修改时间查找（单位：天；0表示24小时内）

`-amin`, `-cmin`, `-mmin` 根据a/c/m的分钟数查找

`+n`, `-n`, `n` 大于，小于，等于n

``` Bash
# 查找当前目录下，文件修改时间在5分钟到24小时之间的文件
$ find -mtime 0 -mmin +5
```

-----

`-name`根据文件名查找，可以使用通配符（使用通配符时要加引号，或使用转义）；`-iname`根据文件名查找（忽略大小写）

`-path`、`-wholename` 全名模式  ？？？

`-regex`正则表达式匹配，`-iregex`忽略大小写的正则表达式匹配。

`-regextype`指定正则表达式类型（如posix-awk）

-----

`-type`根据文件类型查找；`d`目录，`f`普通文件，`l`符号链接

```Bash
# 查找/etc目录下，文件大小大于30k并且为普通文件的文件
$ sudo find /etc -type f -size +30k
```
-----

根据所属查找：

* `-user 用户`
* `-group 组`
* `-uid UID`
* `-gid GID`

根据权限筛选：`-readable`, `-writable`, `-executable`

-----

在路径前的参数：`-P`检查符号链接本身；`-L`检查链接所指向的文件；`-H`不遵循符号链接   ？？？

`-lname`根据名字查找符号链接（`-ilname`不区分大小写）

`find -samefile xxx` 查找当前目录下和xx的inode相同的文件

`find -inum 1234` 查找当前目录下inode号为123的文件

`find -links n` 查找当前目录下硬链接数为n的文件（n也可以是+n、-n）

## locate/whereis/which

`locate`命令利用保存有系统中所有文件名和目录名的数据库中去查找来实现快速查找

* `updatedb` 更新数据库
* `--basename` 根据基础名称查找

`whereis`仅搜索二进制文件或man手册文件

`which`用于查找命令的完整路径。`-a` 列出所有匹配的查找结果

## 文件打包和解压

gzip:

* `gzip 文件` 压缩文件为.gz
* `gunzip 文件` 解压缩.gz文件
* `-k` 压缩或解压缩时保留源文件
* `-v` 显示处理详情

tar:

* `-c` 创建tar
* `-z` 指定gzip格式
* `-v` 显示处理详情
* `-f` 指定文件名称
* `-x` 还原tar


