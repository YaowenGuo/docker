FROM ubuntu

LABEL Description="This image is used to webrtc signaling server." Vendor="GuoYaowen" Version="1.0"

# 避免 tzdata 以交互方式提醒用户选择使用位置卡死构建。
# 以非交互安装。
ENV DEBIAN_FRONTEND=noninteractive
# 或者
# RUN DEBIAN_FRONTEND=noninteractive apt install -y tzdata

RUN apt-get update && apt-get install -y \
iproute2 \
iputils-ping \
curl \
wget \
git \
vim \
npm \
&& curl -sL https://deb.nodesource.com/setup_15.x | bash - \
&& apt-get install -y nodejs

# COPY ./config.json /usr/local/etc/v2ray/

# CMD v2ray --config=/usr/local/etc/v2ray/config.json

# ENV \
# http_proxy=http://127.0.0.1:1087 \
# https_proxy=http://127.0.0.1:1087 \

# echo "
# DEPOT_TOOLS="/opt/webrtc/depot_tools"
# if [ -d $DEPOT_TOOLS]; then
#     export PATH=$PATH:$DEPOT_TOOLS
# fi
# " >> /etc/profile

# source /etc/profile

WORKDIR /opt/webrtc/

# 应用程序提供 443 和 80 服务端口。
EXPOSE 443
EXPOSE 80

