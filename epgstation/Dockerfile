FROM node:14-buster-slim
EXPOSE 8888
ARG CPUCORE="1"
ENV DEV="make gcc git g++ automake curl wget autoconf build-essential python3" 

RUN apt-get update && \
    apt-get -y install $DEV && \
    apt-get -y install openssh-client rsync nkf && \
    apt-get -y install ffmpeg && \
    dpkg --add-architecture i386 && \
    curl -fsSL https://dl.winehq.org/wine-builds/winehq.key | apt-key add - && \
    echo "deb http://dl.winehq.org/wine-builds/debian/ buster main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y install wine-stable=4.0.3~buster wine-stable-amd64=4.0.3~buster wine-stable-i386:i386=4.0.3~buster winehq-stable=4.0.3~buster && \
\
# install EPGStation
    cd /usr/local/ && \
    git clone https://github.com/l3tnun/EPGStation.git && \
    cd /usr/local/EPGStation && \
    npm install --no-save && \
# ffmpeg thumbnail option patch
    sed -i 's/-i %INPUT% -ss ${ thumbnailPosition }/-ss ${ thumbnailPosition } -i %INPUT% -vf yadif=0:-1:1/g' /usr/local/EPGStation/src/server/Model/Operator/Thumbnail/ThumbnailManageModel.ts && \
#
    npm run build && \
\
# 不要なパッケージを削除
\
    apt-get -y remove $DEV && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/EPGStation

ENTRYPOINT npm start

