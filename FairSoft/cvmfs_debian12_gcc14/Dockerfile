FROM gcc:14.2.0-bookworm

ENV NThreads=6
ENV FAIRSOFT_VER="jan24p5"

RUN apt-get autoremove -y &&\
    apt-get update -y && apt-get upgrade -y &&\
    apt-get install -y sudo gfortran- git ninja-build lsb-release libgtest-dev libgmock-dev valgrind ccache curl cmake &&\
    wget https://www.python.org/ftp/python/3.10.16/Python-3.10.16.tgz &&\
    tar -xvf Python-3.10.16.tgz&&\
    cd Python-3.10.16 &&\
    ./configure --prefix=/usr --enable-optimizations --disable-test-modules --enable-shared --with-ensurepip=install &&\
    make -j 4 && make install && cd .. && rm -rf Python-3.10.16 &&\
    apt-get --fix-broken install -y &&\
    apt-get install -y libssl-dev libmaxminddb0 libmaxminddb-dev mmdb-bin cpio uuid-dev libfuse-dev libcap-dev unzip libfuse3-dev &&\
    python3 -m pip config set global.break-system-packages true &&\
    python3 -m pip install -U setuptools &&\
    mkdir -p /misc &&\
    cd /misc &&\
    git clone --depth 1 --branch devel https://github.com/cvmfs/cvmfs.git && cd cvmfs &&\
    mkdir build && cd build && cmake -GNinja .. &&\
    ninja -j${NThreads} && ninja install && cd /misc && rm -rf cvmfs &&\
    cd /misc &&\
    git clone --depth 1 --branch ${FAIRSOFT_VER} https://github.com/FairRootGroup/FairSoft.git FairSoft &&\
    apt-get -y install autoconf automake binutils \
	bison build-essential bzip2 ca-certificates coreutils \
	curl debianutils file findutils flex git gzip \
	hostname libbz2-dev libcurl4-openssl-dev libgsl-dev libicu-dev \
	libfftw3-dev libprotobuf-dev \
	libgl1-mesa-dev libglu1-mesa-dev libgrpc++-dev \
	liblzma-dev libncurses-dev libreadline-dev libsqlite3-dev libssl-dev libtool \
	libx11-dev libxerces-c-dev libxext-dev libxft-dev \
	libxml2-dev libxmu-dev libxpm-dev libyaml-cpp-dev libzstd-dev lsb-release make patch \
	python3-dev protobuf-compiler-grpc rsync sed subversion tar unzip wget xutils-dev xz-utils &&\
    apt-get -y clean &&\
    apt-get remove -y cmake &&\
    ./FairSoft/bootstrap-cmake.sh /usr &&\
    rm -rf FairSoft

CMD ["python3", "-m", "pip", "config", "set", "global.break-system-packages", "true"]

LABEL name="cvmfs_clang15"
LABEL email="yannzhaow@gmail.com"
LABEL author="Yanzhao Wang"

