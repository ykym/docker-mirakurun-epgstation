FROM debian:buster
EXPOSE 8888
ARG CPUCORE="1"
ENV DEV="make gcc git g++ automake curl wget autoconf build-essential python2.7" 

RUN apt-get update && \
    apt-get -y install $DEV && \
    apt-get -y install openssh-client rsync nkf && \
    apt-get -y install ffmpeg && \
    dpkg --add-architecture i386 && \
    curl -fsSL https://dl.winehq.org/wine-builds/winehq.key | apt-key add - && \
    echo "deb http://dl.winehq.org/wine-builds/debian/ buster main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get -y install winehq-stable && \
\
# nodejs install
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y nodejs && \
\
# install EPGStation
    cd /usr/local/ && \
    git clone https://github.com/l3tnun/EPGStation.git && \
    cd /usr/local/EPGStation && \
    npm install && \
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
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/ffmpeg_sources

WORKDIR /usr/local/EPGStation

ENTRYPOINT npm start

