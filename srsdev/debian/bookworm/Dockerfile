FROM debian:bookworm-slim

ENV CMAKE_GENERATOR=Ninja
ENV RELEASE=bookworm

RUN apt-get update &&\
        apt-get install -y cmake wget ninja-build git binutils dpkg-dev g++ gcc libssl-dev libx11-dev libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev libgmp-dev libmpfr-dev libmpc-dev libisl-dev libzstd-dev&&\
        wget https://ftp.gnu.org/gnu/gcc/gcc-14.2.0/gcc-14.2.0.tar.gz && tar -xvf gcc-14.2.0.tar.gz && cd gcc-14.2.0 &&\
        ./configure -v --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu --prefix=/usr/local/ --enable-checking=release --enable-languages=c,c++ --disable-multilib &&\
        make -j6 && make install && cd .. &&\
        echo "deb http://apt.llvm.org/${RELEASE}/ llvm-toolchain-${RELEASE}-19 main" > /etc/apt/sources.list.d/llvm.list &&\
        wget -qO /etc/apt/trusted.gpg.d/llvm.asc https://apt.llvm.org/llvm-snapshot.gpg.key &&\
        apt-get -qq update &&\
        apt-get install -qqy -t llvm-toolchain-${RELEASE}-19 clang-19 &&\
        apt-get update && rm -rf /gcc-14.2.0 /gcc-14.2.0.tar.gz

ENV CC="/usr/lib/llvm-19/bin/clang --gcc-toolchain=/usr/local"
ENV CXX="/usr/lib/llvm-19/bin/clang++ --gcc-toolchain=/usr/local"
