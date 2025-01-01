FROM ubuntu:20.04

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
xz-utils \
&& mkdir -p /opt/webrtc \
&& cd /root


# ENV \
# http_proxy=http://127.0.0.1:1087 \
# https_proxy=http://127.0.0.1:1087
# depot_tools 不能使用 src/third_party 目录下的，否则 gclient sync 版本会有冲突。
RUN echo '\n\n\
export DEPOT_TOOLS=/opt/webrtc/linux_android/src/third_party/depot_tools\n\
if [ -d $DEPOT_TOOLS ]; then\n\
    PATH=$PATH:$DEPOT_TOOLS\n\
fi\n\
export ANDROID_HOME=/opt/webrtc/linux_android/src/third_party/android_sdk\n\
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools\n'\
>> /etc/bashrc

# RUN source /etc/bashrc


# sed 删除指定行
# RUN sed -i '53d' /root/start-ovs.sh
