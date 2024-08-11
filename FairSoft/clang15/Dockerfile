FROM silkeh/clang:15-buster

ENV CC=/usr/bin/gcc
ENV CXX=/usr/bin/g++
ENV SIMPATH=/misc/FairSoft-nov22p1/install
ENV FAIRROOTPATH=/misc/FairRoot
ENV PATH=${SIMPATH}/bin:/misc/cmake/bin:$PATH

 
RUN apt-get autoremove -y &&\
    apt-get update -y && apt-get upgrade -y &&\
    mkdir -p /misc &&\
    cd /misc &&\
    wget https://github.com/FairRootGroup/FairSoft/archive/refs/tags/nov22p1.tar.gz &&\
    tar -xvf nov22p1.tar.gz &&\
    rm nov22p1.tar.gz &&\
    /misc/FairSoft-nov22p1/legacy/setup-debian.sh &&\
    apt-get remove -y cmake &&\
    /misc/FairSoft-nov22p1/bootstrap-cmake.sh /misc/cmake &&\
    cd /misc/FairSoft-nov22p1/ &&\
    mkdir build &&\
    cd build &&\
    cmake .. -C ../FairSoftConfig.cmake &&\
    make -j4 &&\
    rm -rf /misc/FairSoft-nov22p1/build && apt-get autoremove -y &&\
    mkdir -p ${FAIRROOTPATH} && cd /misc &&\
    wget https://github.com/FairRootGroup/FairRoot/archive/refs/tags/v18.8.1.tar.gz &&\
    tar -xvf v18.8.1.tar.gz && rm v18.8.1.tar.gz && cd FairRoot-18.8.1 &&\
    mkdir build && cd build &&\
    cmake -DCMAKE_INSTALL_PREFIX=${FAIRROOTPATH} .. && make -j4 && make install &&\
    cd /misc && rm -rf FairRoot-18.8.1 &&\
    apt-get autoremove -y
