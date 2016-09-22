FROM ubuntu:16.04

MAINTAINER Minh Ha <hi@minh.im>

# ——————————
# Base
# ——————————

RUN \
    apt-get update && apt-get install \
    apt-utils \
    software-properties-common \
    python-software-properties \
    build-essential \
    libssl-dev \
    curl \
    git \
    wget \
    unzip \
    -y --no-install-recommends

# ——————————
# Java
# ——————————

    # Use WebUpd8 PPA
RUN \
    add-apt-repository ppa:webupd8team/java -y && \
    apt-get update -y && \

    # Automatically accept the Oracle license
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get install -y oracle-java8-installer && \
    apt-get install -y oracle-java8-set-default

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# ——————————
# Android
# ——————————

    # Install i386 packages required for running 32 bit Android tools
RUN \
    dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y curl libncurses5:i386 libstdc++6:i386 zlib1g:i386

ENV ANDROID_SDK_VERSION r24.4.1
ENV ANDROID_BUILD_TOOLS_VERSION build-tools-23.0.3,build-tools-23.0.2,build-tools-23.0.1

ENV ANDROID_SDK_FILENAME android-sdk_${ANDROID_SDK_VERSION}-linux.tgz
ENV ANDROID_SDK_URL http://dl.google.com/android/${ANDROID_SDK_FILENAME}
ENV ANDROID_API_LEVELS android-23
ENV ANDROID_EXTRA_COMPONENTS extra-android-m2repository,extra-google-m2repository
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

  # Install Android SDK
RUN cd /opt && \
    wget -q ${ANDROID_SDK_URL} && \
    tar -xzf ${ANDROID_SDK_FILENAME} && \
    rm ${ANDROID_SDK_FILENAME} && \
    echo y | android update sdk --no-ui -a --filter tools,platform-tools,${ANDROID_API_LEVELS},${ANDROID_BUILD_TOOLS_VERSION} && \
    echo y | android update sdk --no-ui --all --filter "${ANDROID_EXTRA_COMPONENTS}"

# ——————————
# Gradle
# ——————————

ENV GRADLE_VERSION 2.14

RUN \
    cd /usr/lib && \
    curl -fl https://downloads.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle-bin.zip && \
    unzip "gradle-bin.zip" && \
    ln -s "/usr/lib/gradle-${GRADLE_VERSION}/bin/gradle" /usr/bin/gradle && \
    rm "gradle-bin.zip"

ENV GRADLE_HOME /usr/lib/gradle
ENV PATH $PATH:$GRADLE_HOME/bin

# ——————————
# Node and global packages
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
# Install Basic React-Native packages
# ——————————

RUN \
    npm install npm -g && \
    npm install react-native-cli -g && \
    npm install rnpm -g

ENV LANG en_US.UTF-8

RUN which android
RUN which adb
RUN node --version
RUN npm --version

# ——————————
# Clean up
# ——————————

RUN \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    apt-get autoremove -y && apt-get clean
