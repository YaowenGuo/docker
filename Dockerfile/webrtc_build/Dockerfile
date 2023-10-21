FROM ubuntu:latest

LABEL Description="This image is used to build webrtc native code or generate android demo project." Vendor="GuoYaowen" Version="0.2"

# 避免 tzdata 以交互方式提醒用户选择使用位置卡死构建。
# 以非交互安装。
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
iproute2 \
iputils-ping \
curl \
wget \
git \
vim \
clang \
systemd \
pkg-config \
file \
&& mkdir -p /opt/webrtc \
&& cd /root


# ENV \
# http_proxy=http://127.0.0.1:1087 \
# https_proxy=http://127.0.0.1:1087

RUN echo "\n\n\
DEPOT_TOOLS="/opt/webrtc/depot_tools"\n\
if [ -d \$DEPOT_TOOLS ]; then\n\
    export PATH=$PATH:\$DEPOT_TOOLS\n\
fi\n\
ANDROID_HOME=/opt/webrtc/linux_android/src/third_party/android_sdk\n\
export PATH=$PATH:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools\n\
"\
>> /etc/bashrc

# RUN source /etc/bashrc


# sed 删除指定行
# RUN sed -i '53d' /root/start-ovs.sh
