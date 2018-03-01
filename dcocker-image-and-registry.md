镜像是docker 用来启动容器的构建基石，那么什么是镜像呢？

# Docker 镜像

> 什么是镜像

Docker 是由文件系统叠加而成.
![docker file system frame](/assets/屏幕快照 2018-02-28 下午4.33.39.png)

最低端是一个引导文件系统：bootfs，这很像典型的Linux/Unix的引导文件系统。Docker用户几乎永远不会和引导文件系统有什么交互，实际上，当一个容器被加载进内存，启动后，引导文件会被卸载。以留出更多的内存供initrd磁盘镜像使用。

Dcoekr 很像一个典型的Linux虚拟化栈，docker镜像的第二层是root文件系统rootfs，它位于引导文件系统之上，rootfs可以是一种或者多种操作系统(如Debian或者readhat)

当引导程序结束并完成完整性检查后，它才会被切换为读写模式。



