1. 创建

```
cd <Dockerfile 所在目录>
docker build -t albertguo88/build_webrtc:latest .

docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v $HOME/project/webrtc/:/opt/webrtc -w /opt/webrtc/linux_android --name webrtc_build albertguo88/build_webrtc /bin/bash

docker exec -it webrtc_build /bin/bash

bash install-dat-release.sh
```

[docker中运行lldb出现错误：process launch failed: 'A' packet returned an error: 8](https://javamana.com/2021/12/202112200008292891.html)

想要使用 lldb 调试，需要给 run 增加 `--cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined` 参数。

2. 进入 docker 配置环境

```
$ DEPOT_TOOLS="/opt/webrtc/depot_tools"
$ export PATH=$PATH:$DEPOT_TOOLS
$ echo $PATH

if [ -d DEPOT_TOOLS ]; then
    export PATH=$PATH:$DEPOT_TOOLS
    echo $PATH
fi
source
```


## 启动代理

## 问题

1. /etc/bashrc 不自动生效。
