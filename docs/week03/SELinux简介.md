# SELinux简介

SELinux配置文件：`/etc/selinux/config`：

``` ini
# disabled: 关闭SELinux
# permissive：当操作越过SELinux设置的权限、策略时，打印警告、记录日志，但不阻止
# enforcing：当操作越过了SELinux设置的权限、策略时，记录日志，并阻止
SELINUX=permissive
# targeted：针对服务进程进行访问控制
# minimum：由target修订而来，针对选择部分的进程保护
# mls：针对所有进程进行多级别访问控制
SELINUXTYPE=targeted
```

修改SELinux配置文件需重启电脑生效。使用`minimum`或`mls`策略需要安装：

``` Bash
$ sudo yum install selinux-policy-minimum selinux-policy-mls
```

- 获取SELinux状态：`$ getenforce`
- 获取SELinux详细状态：`$ sestatus`
- 临时切换SELinux状态（0-Permissive  1-Enforcing）：`$ sudo setenforce 1`

SELinux的日志：

- 若auditd开启，记录到/var/log/audit/audit.log
- 若rsyslog开启而auditd未开启，记录到/var/log/messages
- 若安装了setroubleshoot，且rsyslog和auditd均开启，在/var/log/audit/audit.log中记录，并将其翻译后写入/var/log/messages，可读性更好

安装setrouble：`$ sudo yum install setroubleshoot-server`，然后重启auditd生效。
