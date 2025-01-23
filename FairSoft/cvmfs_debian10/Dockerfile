FROM silkeh/clang:19-buster

ENV NThreads=6
ENV FAIRSOFT_VER="jan24p5"

RUN apt-get autoremove -y &&\
    apt-get update -y && apt-get upgrade -y &&\
    apt-get install -y git ninja-build python3-pip lsb-release libgtest-dev libgmock-dev valgrind ccache curl&&\
    apt-get --fix-broken install -y&&\
    mkdir -p /misc &&\
    cd /misc &&\
    wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb &&\
    dpkg -i cvmfs-release-latest_all.deb &&\
    rm -f cvmfs-release-latest_all.deb &&\
    apt-get update -y && apt-get install -y cvmfs &&\
    git clone --depth 1 --branch ${FAIRSOFT_VER} https://github.com/FairRootGroup/FairSoft.git FairSoft &&\
    ./FairSoft/legacy/setup-debian.sh &&\
    apt-get remove -y cmake &&\
    ./FairSoft/bootstrap-cmake.sh /usr &&\
    rm -rf FairSoft

LABEL name="cvmfs_clang15"
LABEL email="yannzhaow@gmail.com"
LABEL author="Yanzhao Wang"
