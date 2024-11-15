FROM debian:bullseye-slim

ENV CMAKE_GENERATOR=Ninja
ENV RELEASE=bullseye
ENV DOCKER_BUILD_THREAD_NUM=6

RUN apt-get update &&\
        apt-get install -y cmake wget ninja-build git binutils dpkg-dev g++ gcc libssl-dev libx11-dev libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev libgmp-dev libmpfr-dev libmpc-dev libisl-dev libzstd-dev&&\
        mkdir -p root-build&&\
        git clone -b latest-stable --depth=1 https://github.com/root-project/root.git root-src &&\
        cmake -B root-build -S root-src -GNinja -Dxrootd=OFF &&\
        cmake --build root-build --target install -- -j${DOCKER_BUILD_THREAD_NUM} &&\
        wget https://ftp.gnu.org/gnu/gcc/gcc-14.2.0/gcc-14.2.0.tar.gz && tar -xvf gcc-14.2.0.tar.gz && cd gcc-14.2.0 &&\
        ./configure -v --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu --prefix=/usr/local/ --enable-checking=release --enable-languages=c,c++ --disable-multilib &&\
        make -j${DOCKER_BUILD_THREAD_NUM} && make install && cd .. &&\
        echo "deb http://apt.llvm.org/${RELEASE}/ llvm-toolchain-${RELEASE}-19 main" > /etc/apt/sources.list.d/llvm.list &&\
        wget -qO /etc/apt/trusted.gpg.d/llvm.asc https://apt.llvm.org/llvm-snapshot.gpg.key &&\
        apt-get -qq update &&\
        apt-get install -qqy -t llvm-toolchain-${RELEASE}-19 clang-19 &&\
        apt-get update && rm -rf /gcc-14.2.0 /gcc-14.2.0.tar.gz /root-src /root-build

ENV CC="/usr/lib/llvm-19/bin/clang --gcc-toolchain=/usr/local"
ENV CXX="/usr/lib/llvm-19/bin/clang++ --gcc-toolchain=/usr/local"
