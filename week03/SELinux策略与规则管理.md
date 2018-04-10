# SELinux策略与规则管理

查看SELinux上下文：

- `ls -Z` 查看文件、目录的安全上下文（默认情况下，新建的文件或目录会继承其父目录的SELinux类型）
- `ps -Z` 查看进程的安全上下文
- `id -Z` 查看当前用户的安全上下文

安全上下文语法：

``` 
user:role:type:level
```

- user 用户身份，用户会话期间不改变，不同于Linux用户
    - unconfined_u：不受限的用户 
    - system_u：系统用户
- role 被授权于领域（domains），控制对象类型的访问
    - object_r：代表文件或目录等文件资源 
    - system_r：代表程序
- type 决定主体程序能否读取文件资源，以及如何访问和域等规则。当一个类型与执行中的进程相关联时，其type也称为domain

文件的SELinux上下文临时修改：`chcon`：

- `--reference=file` 设置为与file文件相同
- `-u` 设置用户身份
- `-r` 设置角色
- `-t` 设置类型
- `-R` 递归处理文件及子目录

文件的SELinux上下文永久修改：`semanage`：

需要先安装该工具：`$ sudo yum install policycoreutils-python`

