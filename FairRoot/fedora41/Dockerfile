FROM yanzhaowang/r3bdev:fedora41

ENV FAIRROOTPATH="/root/FairRoot"
ENV THREAD_NUM=6
ENV UCESB_DIR="/root/ucesb"

RUN yes | dnf upgrade --refresh &&\
        dnf install -y byacc && pip install conan --upgrade &&\
        mkdir -p $FAIRROOTPATH &&\
        cd /tmp &&\
        git clone --depth 1 --branch dev https://github.com/FairRootGroup/FairRoot.git &&\
        cd FairRoot &&\
        mkdir build && cd build && cmake -GNinja -DCMAKE_INSTALL_PREFIX=${FAIRROOTPATH} .. &&\
        ninja -j${THREAD_NUM} && ninja install &&\
        rm -rf /tmp/FairRoot &&\
        cd /root && git clone https://git.chalmers.se/expsubphys/ucesb.git && cd ucesb && make empty/empty

