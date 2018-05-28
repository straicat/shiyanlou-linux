# Linux日志系统

日志文件位于：`/var/log`

* `alternatives.log`记录系统的更新替代信息
* `dpkg.log`记录`dpkg`命令安装或清除软件包的信息
* `apt`文件夹下，`history.log`记录了`apt`安装或移除软件包的信息，`term.log`记录了安装过程信息
* `wtmp`记录用户登录、注销和系统启动、停机历史，使用`last`命令查看
* `lastlog`记录各用户最后一次登录的信息，使用`lastlog`命令查看

-----

rsyslog主要由Input, Output, Parser三个模块构成。

启动服务：`sudo service rsyslog start`

rsyslog配置文件：

* `/etc/rsyslog.conf` 配置rsyslog模块加载、文件所属者等
* `/etc/rsyslog.d/50-default.conf` 日志设备日志规则

[rsyslog配置官方文档](http://www.rsyslog.com/doc/v8-stable/configuration/index.html)

``` 
# 50-default.conf
kern.*        -/var/log/kern.log
# *表示所有级别的日志信息都写入，路径前的-表示异步写入
```

可以使用`logger`向syslog发送日志。

-----

`logrotate`是一个日志文件管理工具，是基于cron来运行的，配置文件位于`/etc/logrotate.conf`和`/etc/logrotate.d/`。

常用配置参数：

* `monthly` 日志文件按月存储。可以换成`daily`, `weekly`或`yearly`
* `rotate 5` 一次转储5个归档日志
* `compress` 已转储的日志用gzip进行压缩
* `delaycompress` 不会将最近的压缩
* `missingok` 转储时忽略错误
* `notifempty` 如果日志文件为空，不进行转储
* `create 644 root root` 以指定的权限创建新的日志文件，logrotate也会重命名原始日志文件
* `postrotate/endscript` 完成后运行指定的命令

注：编写的位于`logrotate.d`里的配置文件的权限须为`-rw-r--r--`

`logrotate`常用选项：

* `-d` 以预演方式运行
* `-f` 强制进行
* `-v` 详细输出


