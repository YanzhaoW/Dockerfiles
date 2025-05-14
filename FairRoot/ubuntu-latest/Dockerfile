FROM ubuntu:latest

ENV FAIRROOTPATH="/root/FairRoot"
ENV THREAD_NUM=6
ENV UCESB_DIR="/root/ucesb"
ENV SIMPATH="/usr/local"

SHELL ["/bin/bash", "-c"]
RUN apt-get update -y && apt-get upgrade -y &&\
    apt-get install -y sudo git ninja-build python3-pip wget lsb-release libgtest-dev libgmock-dev curl &&\
    mkdir -p /tmp/download ${FAIRROOTPATH} &&\
    cd /tmp/download && git clone --depth 1 --branch dev https://github.com/FairRootGroup/FairSoft.git &&\
    cd FairSoft && source legacy/setup-ubuntu.sh &&\
    mkdir build && cd build &&\
    cmake -GNinja .. && ninja -j6 &&\
    cd /tmp/download/ &&\
    apt-get install -y curl gcc libbz2-dev libev-dev libffi-dev libgdbm-dev liblzma-dev libncurses-dev libreadline-dev libsqlite3-dev libssl-dev tk-dev zlib1g-dev &&\
    wget https://www.python.org/ftp/python/3.10.16/Python-3.10.16.tgz &&\
    tar -xvf Python-3.10.16.tgz &&\
    cd Python-3.10.16 &&\
    ./configure --prefix=/usr --enable-optimizations --disable-test-modules --enable-shared --with-ensurepip=install &&\
    make -j 4 && make install && cd .. && rm -rf Python-3.10.16* &&\
    cd /tmp/download &&\
    git clone --depth 1 --branch dev https://github.com/FairRootGroup/FairRoot.git &&\
    cd FairRoot &&\
    mkdir build && cd build && cmake -GNinja -DCMAKE_INSTALL_PREFIX=${FAIRROOTPATH} .. &&\
    ninja -j${THREAD_NUM} && ninja install &&\
    cd /root && git clone https://git.chalmers.se/expsubphys/ucesb.git && cd ucesb && make empty/empty &&\
    cd / && rm -rf /tmp/download
