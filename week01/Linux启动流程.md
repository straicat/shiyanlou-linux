# Linux启动流程

系统初始化配置文件目录：

* `/etc/init`
* `/etc/rcN.d`
* `/etc/init.d`

在`/etc/rcN.d`目录中定义了各种运行级别的运行服务。K开头的是系统将终止对应的服务，S开的是系统将启动对应的服务。S或K后面的数字时程序优先级，数值越小优先级越高，数字后面是服务名称。

|级别|功能|
|---|---|
|0|关闭系统|
|1|单用户模式|
|2/3/4/5|多用户模式|
|6|重启系统|
|S|单用户模式，文本登录界面，只运行很少几项系统服务|

自启动服务管理工具：`sysv-rc-conf`

使用`update-rc.d`命令修改自启动：

``` Bash
# 添加MySQL自启动（使用默认设置）
$ sudo update-rc.d mysql defaults
# 检查MySQL自启动情况
$ ll /etc/rc?.d/*mysql
# 移除MySQL自启动
$ sudo update-rc.d -f mysql remove
# 设置MySQL在2 3 4级别自启动，0 1 5 6级别不自启动，优先级为50
$ sudo update-rc.d mysql start 50 2 3 4 . stop 50 0 1 5 6 .
```

