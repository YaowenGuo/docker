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

