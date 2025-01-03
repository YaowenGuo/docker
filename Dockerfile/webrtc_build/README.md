1. 创建

```
cd <Dockerfile 所在目录>
docker build -ti --rm --platform linux/amd64 -t albertguo88/build_webrtc:latest .

docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v $HOME/projects/webrtc/:/opt/webrtc -w /opt/webrtc/linux_android --name webrtc_build albertguo88/build_webrtc /bin/bash

docker exec -it webrtc_build /bin/bash

```

[docker中运行lldb出现错误：process launch failed: 'A' packet returned an error: 8](https://javamana.com/2021/12/202112200008292891.html)

想要使用 lldb 调试，需要给 run 增加 `--cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined` 参数。

2. 进入 docker 配置环境

docker exec -it webrtc_build /bin/bash
source /etc/bashrc
gclient sync

## 指定 x86 平台

webrtc 编译目前只支持 x84 的 linux.
docker build -ti --rm --platform linux/amd64 -t albertguo88/build_webrtc:latest .

docker inspect <image id> | grep Architecture

## 问题

1. /etc/bashrc 不自动生效。

docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v /data/pdfium/:/data/pdfium -w /data/pdfium --name build_pdfium albertguo88/build_pdfium:latest /bin/bash

docker exec -it pdfium /bin/bash

docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v $HOME/Projects/linux/:/opt/linux -w /opt/linux --name linux albertguo88/build_webrtc /bin/bash


docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v $HOME/projects/webrtc:/opt/webrtc -w /opt/webrtc/linux_android --name webrtc_build albertguo88/build_webrtc_x86:latest /bin/bash

docker run -itd --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --security-opt apparmor=unconfined -v $HOME/projects/aosp/:/opt/aosp -w /opt/aosp --name aosp_build albertguo88/build_webrtc_x86 /bin/bash