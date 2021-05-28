1. 创建

```
cd <Dockerfile 所在目录>
docker build -t webrtc_build:latest .

docker run -itd -v $HOME/project/webrtc/:/opt/webrtc  --name webrtc_build webrtc_build /bin/bash

docker exec -it webrtc_build /bin/bash

bash install-dat-release.sh
```

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

## 问题

1. /etc/profile 不自动生效。
2. /v2ray 下载不成功。

