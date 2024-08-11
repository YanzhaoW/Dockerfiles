FROM silkeh/clang:15-buster

ENV PATH=/misc/cmake/bin:$PATH
ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"

RUN apt-get autoremove -y &&\
    apt-get update -y && apt-get upgrade -y &&\
    apt-get install -y git lsb-release libgtest-dev valgrind ccache&&\
    apt-get --fix-broken install -y&&\
    mkdir -p /misc &&\
    cd /misc &&\
    wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb &&\
    dpkg -i cvmfs-release-latest_all.deb &&\
    rm -f cvmfs-release-latest_all.deb &&\
    apt-get update -y && apt-get install -y cvmfs &&\
    wget https://github.com/FairRootGroup/FairSoft/archive/refs/tags/nov22p1.tar.gz &&\
    tar -xvf nov22p1.tar.gz &&\
    rm nov22p1.tar.gz &&\
    /misc/FairSoft-nov22p1/legacy/setup-debian.sh &&\
    apt-get remove -y cmake &&\
    /misc/FairSoft-nov22p1/bootstrap-cmake.sh /misc/cmake &&\
    rm -rf FairSoft-nov22p1 &&\
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh &&\
    mkdir /root/.conda &&\
    bash Miniconda3-latest-Linux-x86_64.sh -b &&\
    rm -f Miniconda3-latest-Linux-x86_64.sh &&\
    conda init bash &&\
    /bin/bash -c "source /root/.bashrc" &&\
    pip3 install conan

CMD /bin/bash -c "source /root/.bashrc"

LABEL name="cvmfs_clang15"
LABEL email="yannzhaow@gmail.com"
LABEL author="Yanzhao Wang"
