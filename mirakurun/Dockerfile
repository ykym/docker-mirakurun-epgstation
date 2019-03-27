FROM debian:stretch
EXPOSE 40772
ENV DOCKER="YES"
ENV DEV="libpcsclite-dev build-essential curl libssl-dev yasm libtool autoconf pkg-config gcc git g++ make automake"

ADD ./libaribb25 /usr/local/libaribb25

RUN apt-get update && \
    apt-get -y install pcscd libpcsclite1 libccid pcsc-tools && \
    apt-get -y install $DEV && \
\
# libaribb25
\
    test -f /usr/local/libaribb25/Makefile || git clone https://github.com/epgdatacapbon/libaribb25 /tmp/libaribb25 && cp -rp /usr/local/libaribb25 /tmp/ && \
    cd /tmp/libaribb25 && \
    make clean && \
    make && \
    make install && \
    ldconfig && \
\
# bondriver
\
    mkdir /usr/local/lib/BonDriver/ && \
    git clone https://github.com/u-n-k-n-o-w-n/BonDriverProxy_Linux /tmp/BonDriverProxy_Linux && \
    cd /tmp/BonDriverProxy_Linux && \
    make clean && \
    make client && \
    cp -rp BonDriver_Proxy.so /usr/local/lib/BonDriver/BonDriver_Proxy-S0.so && \
    cp -rp BonDriver_Proxy.so /usr/local/lib/BonDriver/BonDriver_Proxy-T0.so && \
    echo ';OPTION\n\
ADDRESS=bondriverproxy\n\
PORT=1192\n\
BONDRIVER=PT-S\n\
CHANNEL_LOCK=200\n\
CONNECT_TIMEOUT=30\n\
USE_MAGICPACKET=0\n\
\n\
;SYSTEM\n\
PACKET_FIFO_SIZE=32\n\
TS_FIFO_SIZE=64\n\
TSPACKET_BUFSIZE=192512' > /usr/local/lib/BonDriver/BonDriver_Proxy-S0.so.conf && \
    echo ';OPTION\n\
ADDRESS=bondriverproxy\n\
PORT=1192\n\
BONDRIVER=PT-T\n\
CHANNEL_LOCK=200\n\
CONNECT_TIMEOUT=30\n\
USE_MAGICPACKET=0\n\
\n\
;SYSTEM\n\
PACKET_FIFO_SIZE=32\n\
TS_FIFO_SIZE=64\n\
TSPACKET_BUFSRZE=192512' > /usr/local/lib/BonDriver/BonDriver_Proxy-T0.so.conf && \
\
    git clone https://github.com/ykym/recbond /tmp/recbond && \
    cd /tmp/recbond && \
    ./autogen.sh && \
    ./configure --enable-b25 && \
    make clean && \
    make && \
    make install && \
\
# recpt1
\
    git clone https://github.com/stz2012/recpt1 /tmp/recpt1 && \
    cd /tmp/recpt1/recpt1 && \
    sed -i -e 's/arib25/aribb25/g' configure.ac && \
    sed -i -e 's/arib25/aribb25/g' -e 's/HAVE_LIBARIB25/HAVE_LIBARIBB25/g' -e '/#include <aribb25\/arib_std_b25.h>/a#include <aribb25\/arib_std_b25_error_code.h>' decoder.h && \
    sed -i -e 's/HAVE_LIBARIB25/HAVE_LIBARIBB25/g' decoder.c && \
    sed -i -e 's/HAVE_LIBARIB25/HAVE_LIBARIBB25/g' recpt1.c && \
    ./autogen.sh && \
    ./configure --enable-b25 && \
    make && \
    make install && \
\
# nodejs
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y nodejs && \
\
# pm2
\
    npm install pm2 -g && \
\
# arib-b25-stream-test
\
    npm install arib-b25-stream-test -g --unsafe && \
\
# mirakurun
    npm install mirakurun -g --unsafe --production && \
\
# 不要なパッケージを削除
\
    apt-get -y remove $DEV && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/libaribb25 && \
    rm -rf /tmp/BonDriverProxy_Linux && \
    rm -rf /tmp/recbond && \
    rm -rf /tmp/recpt1

WORKDIR /usr/lib/node_modules/mirakurun

ENTRYPOINT /etc/init.d/pcscd start && \
    npm start

