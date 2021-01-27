1. 创建

```
cd <Dockerfile 所在目录>
docker build -t webrtc:latest .

docker run -itd -v /Users/albert/project/webrtc/:/opt/webrtc  --name webrtc webrtc /bin/bash

docker exec -it webrtc /bin/bash

bash install-dat-release.sh
```