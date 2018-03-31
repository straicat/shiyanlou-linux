# Linux任务计划

## at

安装at并启动atd服务：

``` Bash
$ sudo apt-get install at
$ sudo service atd start
```

使用方法：

``` Bash
$ at 时间
at> 待执行的命令
at> <EOT>   # 按Ctrl+d结束，自动出现<EOT>
job 任务号 at 时间
```

at命令需要指定时间，时间格式较为灵活，可以使用以下格式：

* midnight, noon, teatime
* hh:mm today, hh:mm tomorrow。如：14:30 today
* 12小时制，时间后面加上am, pm。如：2:30 am
* 日期与时间。如：14:30 2018/04/01
* 相对计时`now + 时间数量 时间单位`，时间单位可以是minutes, hours, days, weeks。如：now + 3 minutes

* 查看待执行任务列表：`at -l` 或 `atq`
* 删除指定的待执行任务：`at -m 任务号` 或 `atrm 任务号`

`batch` 类似`at`，但在CPU工作负载满足一定条件才执行。

-----

## crontab

* `-e` 编辑crontab
* `-l` 列出crontab
* `-r` 移除所有crontab

|特殊字符|意义|
|---|---|
|`*`|任何|
|`,`|枚举|
|`-`|范围|
|`/`|每隔|

启动crontab（默认是启动的）：`sudo cron -f &`

`%`在crontab里要转义

crontab要注意环境变量，使用绝对路径或设置PATH
