ARG GCC_NAME="15.1.0"
ARG CLANG_VERSION=20
ARG RELEASE=bullseye
ARG BASE_IMAG="debian:bullseye-slim"

FROM ${BASE_IMAG}
ENV CMAKE_GENERATOR=Ninja
ENV DOCKER_BUILD_THREAD_NUM=6

RUN apt-get update &&\
        apt-get install -y cmake wget ninja-build git binutils dpkg-dev g++ gcc libssl-dev libx11-dev libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev libgmp-dev libmpfr-dev libmpc-dev libisl-dev libzstd-dev&&\
        mkdir -p root-build&&\
        git clone -b latest-stable --depth=1 https://github.com/root-project/root.git root-src &&\
        cmake -B root-build -S root-src -GNinja -Dxrootd=OFF &&\
        cmake --build root-build --target install -- -j${DOCKER_BUILD_THREAD_NUM} &&\
        wget https://ftp.gnu.org/gnu/gcc/gcc-${GCC_NAME}/gcc-${GCC_NAME}.tar.gz && tar -xvf gcc-${GCC_NAME}.tar.gz && cd gcc-${GCC_NAME} &&\
        ./configure -v --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu --prefix=/usr/local/ --enable-checking=release --enable-languages=c,c++ --disable-multilib &&\
        make -j${DOCKER_BUILD_THREAD_NUM} && make install && cd .. &&\
        echo "deb http://apt.llvm.org/${RELEASE}/ llvm-toolchain-${RELEASE}-${CLANG_VERSION} main" > /etc/apt/sources.list.d/llvm.list &&\
        wget -qO /etc/apt/trusted.gpg.d/llvm.asc https://apt.llvm.org/llvm-snapshot.gpg.key &&\
        apt-get -qq update &&\
        apt-get install -qqy -t llvm-toolchain-${RELEASE}-${CLANG_VERSION} clang-${CLANG_VERSION} &&\
        apt-get update && rm -rf /gcc-${GCC_NAME} /gcc-${GCC_NAME}.tar.gz /root-src /root-build &&\
        (type -p wget >/dev/null || ( apt update &&  apt-get install wget -y)) \
        &&  mkdir -p -m 755 /etc/apt/keyrings \
        && wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg |  tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
        &&  chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
        && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" |  tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
        &&  apt update \
        &&  apt install gh -y

ENV CC="/usr/lib/llvm-${CLANG_VERSION}/bin/clang --gcc-toolchain=/usr/local"
ENV CXX="/usr/lib/llvm-${CLANG_VERSION}/bin/clang++ --gcc-toolchain=/usr/local"
