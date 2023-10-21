# docker


## 镜像(image)，容器，Docker

- 镜像即一个事物的映像，可以理解为相似，但又不完全相同。这软件上，镜像（Mirroring）是一种文件存储形式，是冗余的一种类型，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像。例如操作系统的安装程序，我们也称为镜像。跟操作系统镜像类似，容器的镜像就是用来启动一个容器实例的文件。

- 容器：就是软件虚拟出来一个软件运行环境，在这里就是镜像运行的实例。（类比于系统镜像运行的操作体统，或者虚拟机。）当我们在容器中安装了许多软件或者配置了自己的环境之后，我们可能希望这个环境能快速的复制给别人使用，或者在另一个地方复制一个，例如线上环境。这时候容器又可以保存为镜像，然后通过保存的镜像启动，就能启动和我们运行的容器一样的容器。

- Docker: Docker 公司开发了自己 docker 软件，用于管理镜像和操作容器。

## 认识容器

> docker 不等于容器

1. docker 使用到了容器技术，“容器”并不是一个新的技术，是在之前就已经存在的。docker使用容器技术，并运用集装箱的设计哲学，将 docker 设计成一个能够将不同软件及其运行环境快速复制到另一环境的工具。这种方式非常类似于运送货物的集装箱，集装箱并不在意内部的内容（货物，在docker中是软件和运行环境），运行和搬运的不再是很多的物品，而是整体打包的集装箱。使得再次的运输和部署非常轻松。

2. docker 不仅含有 docker 容器。docker 可以分为 docker 客户端和 docker 服务端组成。而服务端有包括 docker 容器、守护进程。



## 安装 Docker

使用 Docker Community Edition (CE/EE) app（docker for mac） 要比 boot2docker 安装多一些管理工具

在linux 对应为安装docker-ce/ee

使用这种方法安装mac环境下有一个奇葩的现象，必须先图像化方式启动，然后才有docker 命令可执行。否则执行docker命令时提示没有该指令。


安装完docker之后就可以开始使用了。docker 使用 c/s 架构，因此 docker 软件其实分为两部分，docker 服务端和一个命令行形式的 docker 客户端。这种c/s形式的组合使得 docker 能够应用于云服务和分布式的环境中。

验证docker 是否正确安装且运行

```shell
$ sudo docker info
```

### 连接 docker

c/s 结构的软件都具有一个特点，使用ip地址来建立连接。这个地址通常可以使用localhost 或者 ip 地址。