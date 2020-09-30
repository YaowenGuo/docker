# 快速使用

[TOC]

##  启动 Docker

Docker 是一个 CS 结构的软件，有点类似于 MySQL，使用之前需要先启动。

如果是 Linux 安装的 docker 启动命令一般为
```
sudo service docker start
```
Mac 和 Windows 都有 Docker desktop 图形软件用于启动。

## Image

> 查看本地可用镜像

```shell
docker images
```

首次安装通常没有本地镜像。制作镜像是一个麻烦的过程，不适合新手，既然镜像只是一个文件，我们就可以使用别人制作的镜像。Docker 公司创建了一个仓库，任何组织和个人都可以将制作的镜像分享出来供大家使用。主流的 linux 发行版都有维护官方的 docker 镜像，通过 search 指令即可搜索。

> 通过关键词搜索镜像

例如搜索 ubuntu 镜像。

```shell
docker search ubuntu
```

会列出获得评星最高的几个镜像，通过第一列的名字即可拉取镜像到本地。

```
NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   11351               [OK]
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   468                                     [OK]
rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   248                                     [OK]
consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC session…   226                                     [OK]
ubuntu-upstart                                            Upstart is an event-based replacement for th…   110                 [OK]
.
.
.
```

> 拉取镜像。 

```shell
docker pull ubuntu
```

## 容器

### 查看已有的容器。

```
docker ps -a
```

首次运行应该没有任何容器。

### 创建并启动容器 docker run 命令

run 命令用于从一个镜像创建容器，并运行。

```shell
docker run ubuntu
```

- ubuntu : 基于什么镜像来创建容器，除了ubuntu，还有 fecora、debian、centos等基础镜像。在选择的镜像上创建容器。当执行run命令时，docker首先会在本地查找是否存在该镜像，如果不存在，则会搜索Docker Hub Registry。查看 Docker Hub 中是否有该镜像，Docker 一旦找到该镜像，就会下载到本地宿主机中。然后Docker 会用该镜像修创建一个新容器。该容器拥有自己的网络、IP地址、以及一个用于和宿主机通信的网桥网络接口。

可见，我们可以不必拉取单独拉取镜像，在创建的时候指定镜像名字，就能自动拉取镜像到本地，然后创建容器。

以上指令执行完之后，容器自动退出了。可以通到 `docker ps -l` 查看到最后运行的容器的执行时间。



```
$ sudo docker run -i -t ubuntu /bin/bash
```
- run : docker 的命令
- -i : 保证容器中 STDIN 是开启的。尽管这里没有附着的容器中。
- -t : 交互式的输入是shell的灵魂，-t则是为docker 分配一个伪tty 终端。这样创建的容器才成提供一个交互式的shell。

- /bin/bash : 启动docker执行的命令，这个参数会执行容器中的/bin/bash命令。如果执行该命令，启动后就会有一个容器内的shell可供使用。

执行 /bin/bash 后的容器会有一个交互式的 shell 用于输入。提示会变成类似root@fe9dac62ee46:/# 的输入提示。这就是容器中的shell环境。

> 查看主机名 hostname

容器的主机名就是容器的ID。打开的容器，就是一个完整的Ubuntu系统。



就像一个 Linux 系统一样，我们可以在其中执行任何在 Linux 中执行的程序或者操作。使用 exit 即可退出 `bash` 程序，当程序结束，容器也自动结束了。（我们有其它方法能让容器一直运行，现在先不关注这些）。当容器退出后，当我们编辑文件，或安装一些软件，被安装到哪里了呢？

再次运行 docker，`sudo docker run -i -t ubuntu /bin/bash`。查看我们编辑的文件，或者运行刚刚装过的软件，发现都没了。

```shell
docker ps -a
```

可以看到有多个容器，每次执行 `run` 都会从原始新启动一个容器。而不是从容器运行。运行容器为：

```shell
sudo docker start 容器名 | ID
```

### 启动容器

```shell
sudo docker start 容器名 | ID
```

启动之后，docker 运行我们之前在创建 docker 指定的程序（即 /bin/bash）之后就退出了。那如何进入操作呢？ 可以添加 `-a` 参数(表示附着模式)。

### 进入正在运行的容器

```shell
docker exec -it <容器名 | ID> /bin/bash
```


### 交互进程

> sudo docker exec -t -i <容器名|ID> /bin/bash

- -t -i : 标志执行进程创建TTY，并捕捉STDIN。

- 容器名后面是要执行的命令。
