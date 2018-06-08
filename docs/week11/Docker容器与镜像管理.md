# Docker容器与镜像管理

查看docker帮助：`docker --help`

docker的命令分为Management Commands和Commands。Commands是一些顶级命令，Management Commands将命令进行了分组，更具有可读性。

查看容器列表：`docker container ls`或`docker ps`

- `-a` 显示所有容器
- `-q` 仅显示ID
- `-s` 显示总的文件大小

创建并运行容器：`docker container run 镜像名`或`docker run 镜像名`，相当于先`create`容器，然后`start`容器。

- `-i` 交互模式
- `-t` 分配终端
- `--rm` 退出容器时自动删除
- `-p 主机端口:容器端口` 将容器的端口映射到主机
- `-v` 指定数据卷
- `--env-file 配置文件路径` 使用指定的配置文件运行

退出容器：

- `exit`命令或`Ctrl+q`：程序终止，容器停止
- `Ctrl+p Ctrl+q` 退出容器，容器仍然保持运行

查看容器的详细信息：`docker container inspect 容器`或`docker inspect 容器`，可以通过容器名、容器ID或容器短ID来指定容器

连接到容器：`docker container attach 容器`或`docker attach 容器`

`docker container`还有其它一些命令：

- `log` 获取日志。`-t` 显示时间戳，`-f` 实时输出
- `top` 显示进程
- `diff` 查看修改
- `restart` 重启
- `exec 命令` 执行命令
- `rm` 删除容器

-----

`docker image`系列命令：

- `ls` 查看镜像列表
- `inspect` 查看镜像详细信息
- `pull 镜像名:标签` 拉取镜像
- `rm` 删除镜像

使用Dockerfile构建镜像：

1. 编写Dockerfile
2. 使用Dockerfile构建镜像：`docker build -t 镜像名:标签 .`
