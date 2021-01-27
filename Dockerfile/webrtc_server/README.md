1. 下载 deptools


```shell
docker run -d -p 80:80 --name webrtc_server_1 -v /Users/albert/project/webrtc/:/opt/webrtc -it ubuntu /bin/bash
```

```shell

```

```shell
docker exec -it webrtc_server /bin/bash
```

1. 构建镜像


在当前目录下执行

```
docker build -t webrtc_server:latest  ./
```

或者指定目录

```
docker build -t webrtc_server:latest  /Users/albert/Documents/mkdown/docker/Dockerfile/webrtc_server/
```
