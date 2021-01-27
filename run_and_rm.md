# 创建和删除

可用的参数还有:

- -p <port>: 指定打开的端口号。
- --name <container_name>: 给启动的容器命名
- -d : 以分离（detached）式的方式在后台运行
- -P
- -w : 指定命令的运行工作目录
- -e : 指定环境变量，如 `-e "WEB_PORT=8080"`

**`Docker 的参数是有顺序的，<-d -p -v --name 等参数> <镜像 name> <传给 image 执行的指令>`**

### -p 为容器指定端口

$ sudo docker run -d -p --name <docker_name> <directory_name/image_name:tag> <command>

对外公开 EXPOSE 指令指定的所有端口号，在主句上的端口映射会随机分配。

$ sudo docker run -d -p 80 --name <docker_name> <directory_name/image_name:tag> <command>

指定 容器打开 80 端口，但是在主机内的端口映射却是任意的。

指定映射关系

$ sudo docker run -d -p 8080:80 --name <docker_name> <directory_name/image_name:tag> <command>

将主句的 8080 映射到 docker 容器的 80 端口。

有时候，由于网络的改变，主机的 ip 是不停的变的，此时要想通过 ip 访问一个 容器变得麻烦，只要ip改变，就要改变用于访问容器的 ip 值。其实 docker 也可以指定将端口绑定到数据的什么 ip 上。

$ sudo docker run -d -p 127.0.0.1::80 --name <docker_name> <directory_name/image_name:tag> <command>

这里将容器的 80 端口绑定到本地宿主机的 127.0.0.1 这个 IP

```

$ docker run -d -p 80 --name my_web jamture01/my_docker \
nginx -g "deamon off;"
```

-p 标志用来控制 Docker 在运行时应该公开哪些网络端口给外部（宿主机）。运行一个容器时，Docker可以通过两种方式来在宿主机上分配端口。

1. Docker 可以在宿主机上随机选择一个位于 49153 ~ 65535 的一个比较大的端口号映射到容器80端口上。
2. 可以指定宿主机的一个端口映射到容器的 80 端口上。

上面的运行方式将使用第一中方式，随机分配端口。

```
$ sudo docker run -d -p 8080:80 --name my_web jamture01/my_docker \
nginx -g "deamon off;"
```

将宿主机的 8080 端口绑定到容器的 80 端口上。

也可以将端口绑定限制在特定网络接口（即IP地址）上

```
$ sudo docker run -d -p 127.0.0.1:80:80 --name my_web jamture01/my_docker \
nginx -g "deamon off;"
```

类似的可以将容器的 80 端口绑定到宿主机的一个随机端口上。

```
$ sudo docker run -d -p 127.0.0.1::80 --name my_web jamture01/my_docker \
nginx -g "deamon off;"
```

可以使用 docker inspect 或者 docker port 命令来查看容器内的 80 端口具体被绑定到宿主机哪个端口上。

***也可以在端口绑定时使用/udp后缀来指定 UDP 端口***

-P : 开放在 Dockerfile 中的 EXPOSE 指令中设置的所有端口。

```
$ sudo docker run -d -P --name my_web jamture01/my_docker \
nginx -g "deamon off;"
```

#### 挂载卷

-v: <source_directory>:<target_directory>

-v 将宿主机的目录作为卷挂载到容器里。指定卷的源目录和在容器中的目的目录，通过“:”分割。如果目的目录不存在，docker 会自动创建一个。也可以在目录后面加上 rw 或者 ro 来指定目的目录的读写状态。

```
$ sudo docker run -d -P 80 --name web_site \
-v $PWD/website:/var/www/html/website:ro \
jamture01/my_docker \
nginx -g "deamon off;"
```

**docker 对指令顺序处理并不好，镜像名和执行的指令必须放在最后。**

> 卷： 卷是一个或多个容器内被选定的目录，可以绕过分层的联合文件系统（Union File System），为 Docker 提供持久数据或者共享数据。这意味着对卷的修改会立即生效，并绕过镜像。当提交或者创建镜像时，卷不被包含在镜像里。卷可以在容器间共享，即便是容器停止，卷里的内容依旧存在。

适用卷的场景

- 频繁改动的文件或项目（开发中或测试），比希望重现构建镜像。
- 主机改动后，立即在容器中生效，反之亦然。
- 保存容器中产生的数据
- 多个容器中共享代码。


### 查看宿主机中存在的容器列表 docker ps -a
```
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS                          PORTS                     NAMES
容器id               用于创建该容器的镜像        容器最后执行的命令           创建时间             退出状态                                                   名字
fe9dac62ee46        ubuntu                   "/bin/bash"              14 hours ago        Exited (127) 12 seconds ago                               clever_bassi
```
docker ps 只能查看到正在运行的容器， -a 参数查看所有容器。-l 则只列出最后一次运行的容器，包括正在运行的和停止的。


短UUID 长UUID和名称都能用来指定一个容器。如上面的id就是短的UUID，它是UUID的前一段。

### 容器命名

docker 会为创建的容器随机生成一个名字，如上面的clever_bassi
。如果想要自己指定名字，可以使用--name 表示来实现。

sudo docker run --name bob_the_container -i -t ubuntu /bin/bash

上述创建一个名为 bob_the_container 的容器。一个合法的容器名包括 字母、数字、下换线、圆点、横线。容器的名称必须是唯一的，否则会创建失败。可以使用docker rm 删除已存在的容器。


### 创建守护式容器

守护式容器(daemonized container)没有交互式会话，非常适合在后台运行应用程序和服务。大多数情况都是创建守护式的容器。

```shell
sudo docker run --name daemon_dave -d  ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

-d 参数创建的容器将会在后台运行。容器执行了一个bash 死循环，不停地打印输出。直到容器或者进程停止运行。

使用该命令创建的容器只返回了一个容器ID，而不是交互式的shell。


### 删除容器 docker rm

> docker rm <容器名|ID>

运行中的容器无法删除。没有删除全部容器的指令，可以通过如下技巧

> docke rm `docker ps -a -q`

-a : 所有容器，
-q : 只返回容器的id，而不返回其他信息。