# Apache配置入门

安装（在Ret Hat系为httpd）：

```Bash
$ sudo apt install apache2
```

配置文件位于`/etc/apache2/`，结构：

- apache2.conf  主配置文件
- ports.conf  监听端口配置
- envvars  环境变量配置
- magic  判断文件MIME类型的配置文件
- conf  指令配置
- mods  模块配置
- site  站点配置

修改配置后要reload:

```Bash
$ sudo service apache2 reload
```

【主配置文件】

\<Directory\>：配置某目录的访问

- Indexes: 若没有index.html则展示文件列表
- FollowSymLinks: 允许读取目录中的符号链接
- AllowOverride: 在使用rewrite模块重写时能否读取`.htaccess`中的重写规则
- Require: 访问控制
    - Require all granted 允许所有来源访问
    - Require all denied  拒绝所有来源访问
    - Require ip x.x.x.x  允许IP为x.x.x.x的来源访问

使用正则表达式匹配目录：

- <Directory \~ "REGEXP"\>
- <DirectoryMatch "REGEXP"\>
 
【端口配置】

`Listen` 监听的端口

【站点配置】

所有站点信息都是通过\<VirtualHost\>来包含，配置多个\<VirtualHost\>即可创建多个虚拟主机，访问多个虚拟主机可以通过这样的方式来区分：

- IP地址
- 监听的端口号
- 访问的Servername

在sites-available中创建新的配置文件（一般以.conf结尾）后，运行：

```Bash
$ sudo a2ensite xxx.conf
```

来让Apache创建一个软链接到sites-enable中，这样Apache才会读取相关的配置文件。

【模块配置】

类似的，使用`a2enmod`命令来启用模块。

Apache安全配置：

envvars的`APACHE_RUN_*`配置了Apache运行的用户、用户组、主目录，默认以`www-data`用户运行。

在`conf-available/security.conf`设置`ServerSignature`为Off，在错误页面隐藏Apache和系统的版本信息。

Apache自带的用户认证：

安装`apache2-utils`，使用`htpasswd`创建密码文件，在`apache2.conf`的\<Directory\>配置AuthType、AuthUserFile等信息：

```
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        AuthType basic
        AuthName "need auth"
        AuthUserFile /var/www/.htpasswd
        Require valid-user
</Directory>
```
