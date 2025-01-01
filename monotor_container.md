### 容器内部都在干什么 docker log

上面创建的守护式容器在后台执行，为了查看容器内部在干什么，可以查看日志文件。

```shell
sudo docker logs daemon_dave
```

此时，只会输出最后几条日志并返回。也可以使用 -f 参数来监控容器的日志，这与 tail -f 命令非常类似，

```shell
sudo docker logs -f <容器名|ID>
```

Ctrl + C 退出监控。

监控某一片段，和之前类似，只需要在tail命令之后加入-f --lines标志即可

> docker logs --tail 10 <容器名|ID>  获取日志最后10行内容

> docker logs --tail 0 -f <容器名|ID>  跟踪容器最新的日志而不必读取整个日志文件。

还可以使用-t给每条日志加上时间戳

> sudo docker logs -ft <容器名|ID>

### 查看容器内的进程 docker top

> sudo docker top <容器名|ID>

### 在容器内部运行进程 docker exec

在容器内部额外启动新进程。分为后台和交互两种

### 后台

> sudo docker exec -d <容器名|ID> touch /dec/new_config_file

- -d ： 表名需要运行一个后台进程，

- 容器名后面是要执行的命令。




### 更多容器信息 docker inspect

> sudo docker inspect <容器名|ID>

docker inspect 会对容器进行详细的检查，然后返回其配置信息，包括名称、命令、网络配置以及很多有用的数据。


我们也可以使用-f或者--format来选定查看结果，如：
> sudo docker inspect --format='{{ .State.Running }}' <容器名|ID>

上述命令会返回命令的运行状态，-f或者--format 远没有表面看上去那么简单。该标志支持完整的Go语言模板。

上述的容器名部分还可以指定多个容器，只需要以空格分割即可。

**查看/var/lib/docker目录可以深入了解docker的工作原理，该目录下存放着Docker镜像、容器以及容器配置。所有的容器都保存在/var/lib/docker/containers目录下。**

### 查看容器端口
```shell
docker port
```

或者直接查看哪个端口的映射情况，

sudo docker port <container_name> 80
