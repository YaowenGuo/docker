### 启动容器

```shell
sudo docker start 容器名 | ID
```

启动之后，docker 运行我们之前在创建 docker 指定的程序（即 /bin/bash）之后就退出了。那如何进入操作呢？ 可以添加 `-a` 参数(表示附着模式)。

### 进入正在运行的容器

```shell
docker exec -it <容器名 | ID> /bin/bash
```


### 重新启动已经停止的容器 start

sudo docker start 容器名 | ID

docker restart 重启一个容器。

启动后发现，默认并没有打开交互式的shell。想要交互，则需要附着到该shell上。

### 附着到容器上 attach

容器重新启动的时候，会沿用docker run命令指定的参数来运行。因此docker启动的时候回运行一个交互式的shell会话。此外，还可以使用 docker attach 命令重新附着到该容器会话上。

sudo docker attach 容器名 | ID

**按下回车才会进入会话**


### 自动重启容器

容器中的进程停止了，容器也会随之结束。如果容器中的进程因为错误而终止，可以通过--restart标志重新启动该容器。--restart 会检查容器的退出代码，并据此决定是否要重启容器。

> sudo docker run --restart=always --name test -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"

- always 无论退出代码是什么，docker都会自动重启该容器
- on-failure 退出代码为非0时重启。另外，on-failure还可以接受一个可选的重启次数，如--restart=on-failure:5 最多重启5次。

### 停止守护进程容器 docker stop

> sudo docker stop <容器名|ID>

docker stop 命令会向容器发送SIGTERM信号。如果想要快速停止某个容器，也可以使用docker kill命令来向容器发送SIGKILL信号。

想要查看已经停止的容器的状态，则可以使用docker ps命令。docker ps
 -n x命令则会显示最后x个容器，不论这些容器是在运行还是已经停止。

