FROM silkeh/clang:19-bookworm

ENV NThreads=6
ENV FAIRSOFT_VER="jan24p5"

RUN apt-get autoremove -y &&\
    apt-get update -y && apt-get upgrade -y &&\
    apt-get install -y git ninja-build python3-pip lsb-release libgtest-dev libgmock-dev valgrind ccache curl &&\
    apt-get --fix-broken install -y&&\
    apt-get install -y libssl-dev libmaxminddb0 libmaxminddb-dev mmdb-bin cpio uuid-dev libfuse-dev libcap-dev unzip libfuse3-dev &&\
    python3 -m pip config set global.break-system-packages true &&\
    pip install -U setuptools &&\
    mkdir -p /misc &&\
    cd /misc &&\
    git clone --depth 1 --branch devel https://github.com/cvmfs/cvmfs.git && cd cvmfs &&\
    mkdir build && cd build && cmake -GNinja .. &&\
    ninja -j${NThreads} && ninja install && cd /misc && rm -rf cvmfs &&\
    git clone --depth 1 --branch ${FAIRSOFT_VER} https://github.com/FairRootGroup/FairSoft.git FairSoft &&\
    ./FairSoft/legacy/setup-debian.sh &&\
    apt-get remove -y cmake &&\
    ./FairSoft/bootstrap-cmake.sh /usr &&\
    rm -rf FairSoft

CMD ["python3", "-m", "pip", "config", "set", "global.break-system-packages", "true"]

LABEL name="cvmfs_clang15"
LABEL email="yannzhaow@gmail.com"
LABEL author="Yanzhao Wang"
