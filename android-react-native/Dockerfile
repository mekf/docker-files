FROM mekf/android-base:latest

MAINTAINER Minh Ha <hi@minh.im>

# ——————————
# node base
# ——————————

ENV NODE_VERSION 6.5.0

RUN \
    cd && \
    wget -q http://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz && \
    tar -xzf node-v${NODE_VERSION}-linux-x64.tar.gz && \
    mv node-v${NODE_VERSION}-linux-x64 /opt/node && \
    rm node-v${NODE_VERSION}-linux-x64.tar.gz

ENV PATH ${PATH}:/opt/node/bin

# ——————————
# react-native-cli & npm packages
# ——————————

RUN \
    npm install react-native-cli -g && \
    npm install rnpm -g

ENV LANG en_US.UTF-8

RUN which android
RUN which adb
RUN node --version
RUN npm --version
