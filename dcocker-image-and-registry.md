镜像是docker 用来启动容器的构建基石，那么什么是镜像呢？

# Docker 镜像

> 什么是镜像

Docker 是由文件系统叠加而成.
![docker file system frame](/assets/屏幕快照 2018-02-28 下午4.33.39.png)

最低端是一个引导文件系统：bootfs，这很像典型的Linux/Unix的引导文件系统。Docker用户几乎永远不会和引导文件系统有什么交互，实际上，当一个容器被加载进内存，启动后，引导文件会被卸载。以留出更多的内存供initrd磁盘镜像使用。

Dcoekr 很像一个典型的Linux虚拟化栈，docker镜像的第二层是root文件系统rootfs，它位于引导文件系统之上，rootfs可以是一种或者多种操作系统(如Debian或者readhat)

在传统的Linux引导过程中，root文件系统会最先以只读的方式加载，当引导结束并完成了安全性检查，它才会被切换为读写模式。但是在docker里，root文件系统永远只能是只读状态，，并且Docker利用联合加载（union mount）技术。又会在root文件系统之上加载更多的只读文件系统。联合加载是指一次同时加载多个文件系统。但是外面只能看见一个文件系统。联合加载会将各层文件系统叠加在一起，这样最终的文件系统会包含所有底层的文件系统和目录。

   Dcoekr将这样的文件系统称为镜像。一个镜像可以放到另一个镜像的顶部。位于下面的镜像称为父镜像，以此类推，最底层的镜像称为基础镜像。最后，当从一个镜像启动容器时，Dcoker会在该镜像最顶层加载一个读写文件系统。我们想要在docker中运行的程序就是在这个读写层中执行的。
   
   当Docker第一次启动一个容器时，初始的读写层是空的。当文件系统发生变化时，这些变化都会应用到这一层上。比如想要修改一个文件，这个文件会从只读层复制到该读写层。这种机制被称为写时复制（copy on write）
   
   
   
# 列出镜像

> sudo docker images

该命令会列出所有的dcoker镜像，这些镜像就是执行docker run指令时，从docker官方上下载的。本地的镜像存放在/var/lib/docker目录下。每个镜像都保存在docker所采用的存储驱动目录下面，如aufs或者devicemapper。也可以在/var/lib/docker/containers目录下看到所有的容器。

   镜像从仓库中下载下来，默认的仓库是docker公司运营的公共Registry服务。及Docker Hub。也可以自己建立Registry仓库。
   
   
每个仓库都可以存放很多镜像，比如ubuntu仓库，下拉仓库

> sudo docker pull ubuntu

拉取docker中的所有内容。使用docke images就可以查看所有下拉下来的镜像。

**虽然称其为ubuntu系统，但实际上并不是一个完整的系统。只是一个包换最低限度的支持系统运行的组件。**

同一个仓库中的不认同镜像可以使用tag区分。想要指定某个仓库中的特殊镜像时，就是用用<仓库名:tag>来表示的。

> sudo docker run -it --name new_container ubuntu:12.04 bash

如果没有指定标签，则会默认拉取标记为latest的镜像。

每个镜像可以打几个不同的tag，每个tag实际上是对组成镜像的一个镜像层的标记。例如不通基于ubuntu的镜像都可以打上ubuntu的tag。


docker Hub 包含两种仓库，用于仓库和顶级仓库。顶级仓库是由Docker内部管理的。用户仓库的命名由<用户名/仓库名>两部分组成。而顶级仓库只包含仓库名，如ubuntu。


### 查看指定仓库的镜像

> docker images <仓库名>
docker images fedora

所列出的镜像中，相同id号的镜像是同一镜像，只是tag不同。

# 查找Docker Hub上的镜像

> docker search puppet
查找到之后就可以使用镜像名下载或者创建本机镜像。docker会自动从Dcoker Hub上下载并创建本地镜像。

# 构建镜像
并不是真的从零创建一个新的镜像，而是使用一个已有的基础镜像，如ubuntu构建。如果想要从零构建一个全新的镜像，可以参考:https://docs.docker.com/articles/baseeimages/

构建docker镜像有两种方法
- docker commit 命名
- docker build命令和Dcokerfile文件

不推荐使用docker commit命名，而应该使用更灵活、更强大的dockerfile来构建docker镜像。


### 注册Docker Hub账号

构建系统很重要的一环就是共享和发布，因此可以先初测一个Docker Hub账号，然后就可以使用Dcoker login命令登录了。

个人的信息都会保存在~/.dockercfg文件中。

### commit创建镜像
一般步骤是:
1. docker run 基于已有的镜像创建一个容器。
2. 运行容器，在容器中安装需要的软件和服务。例如ubuntu使用apt命名安装软件。
3. 退出容器。docker commit 执行变更。就像是对git仓库提交一样，将修改提交为一个新镜像。

> sudo docker commit <容器名|ID[:标签名]> <目标仓库/镜像名> [-m="说明" --author="作者"]

ID，可以通过 docekr ps -l -q 得到刚创建的容器的id。
目标仓库: 创建的镜像要保存的仓库
镜像名: 保存的镜像的名字。

提交和git一样，只会保存创建的容器和当前容器之间的差异。


### Dockerfile 构建镜像

Dockerfile使用基于 DSL 的语法来构架一个 Docker 镜像，使用docker build命令来执行Dcokerfile中的指令构建一个新的镜像。









