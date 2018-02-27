安装完docker之后就可以开始使用了。docker 使用 c/s 架构，因此 docker 软件其实分为两部分，docker 服务端和一个命令行形式的 docker 客户端。这种c/s形式的组合使得 docker 能够应用于云服务和分布式的环境中。

> docker 不等于容器

1. docker 使用到了容器技术，“容器”并不是一个新的技术，是在之前就已经存在的。docker使用容器技术，并运用集装箱的设计哲学，将 docker 设计成一个能够将不同软件及其运行环境快速复制到另一环境的工具。这种方式非常类似于运送货物的集装箱，集装箱并不在意内部的内容（货物，在docker中是软件和运行环境），运行和搬运的不再是很多的物品，而是整体打包的集装箱。使得再次的运输和部署非常轻松。

2. docker 不仅含有 docker 容器。docker 可以分为 docker 客户端和 docker 服务端组成。而服务端有包括 docker 容器、守护进程。


### 连接 docker

c/s 结构的软件都具有一个特点，使用ip地址来建立连接。这个地址通常可以使用localhost 或者 ip 地址。但是，如果是在windows或者mac上连接，由于是在 boot2ocker 运行的 docker。boot2docker 其实是一个本地的虚拟机，拥有自己的ip地址和端口号，因此要连接的其实是 boot2docker 的ip地址和端口号，而不是宿主机的。

> 查看 boot2docker 的ip地址

当启动或者安装 boot2docker 的时候，会有提示设置DOCKER_HOST的值，改值存储的就是ip地址和端口号。
$ boot2docker start
或者运行 boot2docker的ip 命令来查看端口号。
$ boot2docker ip

###  启动 Docker
如果是使用 boot2docker 的docker，启动命令为
boot2docker start

如果是 Linux 安装的 docker 启动命令一般为
sudo service docker start

如果不是，请查看 Linux 的系统服务的启动软件是什么，应用响应的启动方式即可。

### 验证docker 是否正确安装且运行

$ sudo docker info


### 创建并启动 容器 docker run 命令

$ sudo docker run -i -t ubuntu /bin/bash

- run : docker 的命令
- -i : 保证容器中 STDIN 是开启的。尽管这里没有附着的容器中。
- -t : 交互式的输入是shell的灵魂，-t则是为docker 分配一个伪tty 终端。这样创建的容器才成提供一个交互式的shell。
- ubuntu : 基于什么镜像来创建容器，除了ubuntu，还有recora、debian、centos等基础镜像。在选择的镜像上创建容器。当执行run命令时，docker首先会在本地查找是否存在该镜像，如果不存在，则会搜索Docker Hub Registry。查看 Docker Hub 中是否有该镜像，Docker 一旦找到该镜像，就会下载到本地宿主机中。然后Docker 会用该镜像修创建一个新容器。该容器拥有自己的网络、IP地址、以及一个用于和宿主机通信的网桥网络接口。
- /bin/bash : 启动docker执行的命令，这个参数会执行容器中的/bin/bash命令。如果执行该命令，启动后就会有一个容器内的shell可供使用。

### 使用容器

执行 /bin/bash 后的容器会有一个交互式的shell用于输入。提示会变成类似root@fe9dac62ee46:/# 的输入提示。这就是容器中的shell环境。

> 查看主机名 hostname

容器的主机名就是容器的ID。打开的容器，就是一个完整的Ubuntu系统。

> 查看 /etc/hosts 文件

127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.2	fe9dac62ee46  # docker 已为该容器添加了一条主机配置项。

再看看容器的网络配置情况。

> 查看网络配置 # ip a
如果没有，看下一条，安装

```
# lo 的换回接口
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1
    link/ipip 0.0.0.0 brd 0.0.0.0
3: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN group default qlen 1
    link/tunnel6 :: brd ::
# 标准 eth0 网络接口。
100: eth0@if101: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```

> 安装一些常用的命令

apt-get update
//ifconfig 
//apt install net-tools  ifconfig 已经好多年没有维护了，你需要使用ip命令。
// ip 套件
apt install iproute2
//ping
apt install iputils-ping 
// 

> 查看进程
ps -aux

### 退出容器 exit

一旦退出了shell， 容器也就随之停止了。

### 查看宿主机中存在的容器列表 docker ps -a
```
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS                          PORTS                     NAMES
容器id               用于创建该容器的镜像        容器最后执行的命令           创建时间             退出状态                                                   名字
fe9dac62ee46        ubuntu                   "/bin/bash"              14 hours ago        Exited (127) 12 seconds ago                               clever_bassi
```
docker ps 只能查看到正在运行的容器， -a 参数查看所有容器。-l 则只列出最后一次运行的容器，包括正在运行的和停止的。


短UUID 长UUID和名称都能用来指定一个容器。如上面的id就是短的UUID，它是UUID的前一段。

# 容器命名

docker 会为创建的容器随机生成一个名字，如上面的clever_bassi
。如果想要自己指定名字，可以使用--name 表示来实现。

sudo docker run --name bob_the_container -i -t ubuntu /bin/bash

上述创建一个名为 bob_the_container 的容器。一个合法的容器名包括 字母、数字、下换线、圆点、横线。容器的名称必须是唯一的，否则会创建失败。可以使用docker rm 删除已存在的容器。

# 重新启动已经停止的容器 start

sudo docker start 容器名 | ID

docker restart 重启一个容器。

启动后发现，默认并没有打开交互式的shell。想要交互，则需要附着到该shell上。

# 附着到容器上 attach

容器重新启动的时候，会沿用docker run命令指定的参数来运行。因此docker启动的时候回运行一个交互式的shell会话。此外，还可以使用 docker attach 命令重新附着到该容器会话上。

sudo docker sttach 容器名 | ID

**按下回车才会进入会话**

# 创建守护式容器

守护式容器(daemonized container)没有交互式会话，非常适合在后台运行应用程序和服务。大多数情况都是创建守护式的容器。

sudo docker run --name daemon_dave -d  ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

-d 参数创建的容器将会在后台运行。容器执行了一个bash 死循环，不停地打印输出。知道容器或者进程停止运行。

使用该命令创建的容器只返回了一个容器ID，而不是交互式的shell。

# 容器内部都在干什么

上面创建的守护式容器在后台执行，为了查看容器内部在干什么，可以查看日志文件。

sudo docker logs daemon_dave

此时，只会输出最后几条日志并返回。也可以使用 -f 参数来监控容器的日志，这与 tail -f 命令非常类似，

> sudo docker logs -f <容器名|ID>

Ctrl + C 退出监控。

监控某一片段，和之前类似，只需要在tail命令之后加入-f --lines标志即可

> docker logs --tail 10 <容器名|ID>  获取日志最后10行内容

> docker logs --tail 0 -f <容器名|ID>  跟踪容器最新的日志而不必读取整个日志文件。

还可以使用-t给每条日志加上时间戳 

> sudo docker logs -ft <容器名|ID>

# 查看容器内的进程 docker top

> sudo docker top <容器名|ID>

# 在容器内部运行进程 docker exec

在容器内部额外启动新进程。分为后台和交互两种

### 后台 

> sudo docker exec -d <容器名|ID> touch /dec/new_config_file

- -d ： 表名需要运行一个后台进程，

- 容器名后面是要执行的命令。

### 交互进程

> sudo docker exec -t -i <容器名|ID> /bin/bash

- -t -i : 标志执行进程创建TTY，并捕捉STDIN。

- 容器名后面是要执行的命令。


# 停止守护进程容器 docker stop

> sudo docker stop <容器名|ID>

docker stop 命令会向容器发送SIGTERM信号。如果想要快速停止某个容器，也可以使用docker kill命令来向容器发送SIGKILL信号。

想要查看已经停止的容器的状态，则可以使用docker ps命令。docker ps
 -n x命令则会显示最后x个容器，不论这些容器是在运行还是已经停止。
 

# 自动重启容器

容器中的进程停止了，容器也会随之结束。如果容器中的进程因为错误而终止，可以通过--restart标志重新启动该容器。--restart 会检查容器的退出代码，并据此决定是否要重启容器。

> sudo docker run --restart=always --name test -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

- always 无论退出代码是什么，docker都会自动重启该容器
- on-failure 退出代码为非0时重启。另外，on-failure还可以接受一个可选的重启次数，如--restart=on-failure:5 最多重启5次。


# 更多容器信息 docker inspect

> sudo docker inspect <容器名|ID>

docker inspect 会对容器进行详细的检查，然后返回其配置信息，包括名称、命令、网络配置以及很多有用的数据。


我们也可以使用-f或者--format来选定查看结果，如：
> sudo docker inspect --format='{{ .State.Running }}' <容器名|ID>

上述命令会返回命令的运行状态，-f或者--format 远没有表面看上去那么简单。该标志支持完整的Go语言模板。

上述的容器名部分还可以指定多个容器，只需要以空格分割即可。

**查看/var/lib/docker目录可以深入了解docker的工作原理，该目录下存放着Docker镜像、容器以及容器配置。所有的容器都保存在/var/lib/docker/containers目录下。**


# 删除容器 docker rm

> docker rm <容器名|ID>

运行中的容器无法删除。没有删除全部容器的指令，可以通过如下技巧

> docke rm `docker ps -a -q`
 
-a : 所有容器，
-q : 只返回容器的id，而不返回其他信息。




