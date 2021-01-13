FROM webrtc_server:0.1

LABEL Description="This image is used to webrtc signaling server." Vendor="GuoYaowen" Version="1.0"


RUN apt-get update && apt-get install -y \
iproute2 \
iputils-ping \
curl \
wget \
git \
vim \
npm \

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





