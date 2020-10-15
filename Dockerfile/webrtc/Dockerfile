FROM webrtc:0.1

LABEL Description="This image is used to build webrtc native code or generate android demo project." Vendor="GuoYaowen" Version="1.0"


RUN apt-get update && apt-get install -y \
iproute2 \
iputils-ping \
curl \
wget \
git \
vim \
clang \
&& mkdir -p /opt/webrtc \
&& mkdir -p /usr/local/share/v2ray \
&& cd /root \
&& bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) \
&& bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh) \

RUN bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)

COPY ./config.json /usr/local/etc/v2ray/

CMD v2ray --config=/usr/local/etc/v2ray/config.json

ENV \
http_proxy=http://127.0.0.1:1087 \
https_proxy=http://127.0.0.1:1087 \

echo "
DEPOT_TOOLS="/opt/webrtc/depot_tools"
if [ -d $DEPOT_TOOLS]; then
    export PATH=$PATH:$DEPOT_TOOLS
fi
" >> /etc/profile

source /etc/profile





