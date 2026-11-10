FROM node:14-buster-slim
EXPOSE 8888
ARG CPUCORE="1"
ENV DEV="make gcc git g++ automake curl wget autoconf build-essential python3 python3-yaml" 

RUN sed -i 's/deb.debian.org/archive.debian.org/g' /etc/apt/sources.list && \
    sed -i 's|security.debian.org/debian-security|archive.debian.org/debian-security|g' /etc/apt/sources.list && \
    sed -i '/stretch-updates/d' /etc/apt/sources.list

RUN apt-get update && \
    apt-get -y install $DEV && \
    apt-get -y install openssh-client rsync nkf && \
    apt-get -y install ffmpeg && \
    apt-get -y install cmake libboost-all-dev && \
    dpkg --add-architecture i386 && \
    curl -fsSL https://dl.winehq.org/wine-builds/winehq.key | apt-key add - && \
    echo "deb http://dl.winehq.org/wine-builds/debian/ buster main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y install wine-stable=4.0.3~buster wine-stable-amd64=4.0.3~buster wine-stable-i386:i386=4.0.3~buster winehq-stable=4.0.3~buster && \
\
# install tsdivider
    git clone https://github.com/tobitti0/tsdivider /tmp/tsdivider && \
    cd /tmp/tsdivider && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release .. && \
    make && \
    mv tsdivider /usr/local/bin/ && \
# install EPGStation
    cd /usr/local/ && \
    git clone https://github.com/l3tnun/EPGStation.git && \
    cd /usr/local/EPGStation && \
    git checkout v1 && \
    npm install npm@6 -g && \
    npm install --no-save && \
    npm run build && \
\
# 不要なパッケージを削除
\
    #apt-get -y remove $DEV && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/tsdivider

WORKDIR /usr/local/EPGStation

ENTRYPOINT npm start

