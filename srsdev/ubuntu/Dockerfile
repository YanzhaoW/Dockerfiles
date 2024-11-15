FROM ubuntu:22.04

ENV CMAKE_GENERATOR=Ninja
ENV CC=clang
ENV CXX="clang++ -stdlib=libc++"

# RUN apt-get update &&\
#         apt install -y binutils cmake dpkg-dev g++ gcc libssl-dev git libx11-dev\
#         libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev wget zlib1g zlib1g-dev ninja-build&&\
#         mkdir -p llvm-build root-build abseil-build abseil-install protobuf-build boost-build&&\
#         git clone -b llvmorg-19.1.3 --depth=1 https://github.com/llvm/llvm-project.git &&\
#         cmake -DLLVM_ENABLE_PROJECTS=clang -DCMAKE_INSTALL_PREFIX=/usr -DLLVM_ENABLE_RUNTIMES="libcxx;libcxxabi;libunwind" -DCMAKE_BUILD_TYPE=Release -DLLVM_LIBDIR_SUFFIX=64 -DLIBCXX_LIBDIR_SUFFIX=64 -DLLVM_ENABLE_LIBCXX=ON -GNinja -S llvm-project/llvm -B llvm-build &&\
#         cmake --build llvm-build --target install -- -j6
        # git clone -b latest-stable --depth=1 https://github.com/root-project/root.git root-src &&\
        # cmake -DCMAKE_INSTALL_PREFIX=/usr -B root-build -S root-src -GNinja -Dxrootd=OFF &&\
        # cmake --build root-build --target install -- -j6 &&\
        # wget https://archives.boost.io/release/1.82.0/source/boost_1_82_0.tar.gz &&\
        # tar -xf boost_1_82_0.tar.gz &&\
        # cd boost_1_82_0 &&\
        # ./bootstrap.sh --with-libraries="thread" --prefix=/usr &&\
        # ./b2 link=static -a &&\
        # ./b2 install &&\
        # cd .. &&\
        # git clone -b 20240722.0 https://github.com/abseil/abseil-cpp.git &&\
        # cmake -B ./abseil-build -S abseil-cpp -GNinja -DCMAKE_INSTALL_PREFIX=./abseil-install -DABSL_PROPAGATE_CXX_STD=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_STANDARD=20 &&\
        # cmake --build ./abseil-build --target install -- -j6 &&\
        # git clone -b v29.0-rc1 --depth=1 https://github.com/protocolbuffers/protobuf.git &&\
        # cd protobuf && git submodule update --init --recursive && cd .. &&\
        # cmake -B ./protobuf-build -S protobuf -GNinja -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_RPEFIX_PATH=./abseil-install -Dprotobuf_LOCAL_DEPENDENCIES_ONLY=ON -DABSL_PROPAGATE_CXX_STD=ON -Dprotobuf_WITH_ZLIB=ON &&\
        # cmake --build ./protobuf-build --target install -- -j6
        # rm -rf llvm-project llvm-build root-src root-build abseil-build abseil-install protobuf-build boost-build boost_1_82_0 abseil-cpp protobuf

Run apt-get update &&\ 
apt install -y binutils cmake dpkg-dev g++ gcc libssl-dev git libx11-dev\
        libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev wget zlib1g zlib1g-dev ninja-build&&\
        mkdir -p llvm-build root-build abseil-build abseil-install protobuf-build boost-build&&\
        echo "deb http://apt.llvm.org/jammy/ llvm-toolchain-jammy-19 main" > /etc/apt/sources.list.d/llvm.list &&\
        wget -qO /etc/apt/trusted.gpg.d/llvm.asc https://apt.llvm.org/llvm-snapshot.gpg.key &&\
        apt-get -qq update &&\
        apt-get install -qqy -t llvm-toolchain-jammy-19 clang-19 clang-tidy-19\
        clang-format-19 lld-19 libc++-19-dev libc++abi-19-dev &&\
        for f in /usr/lib/llvm-*/bin/*; do ln -sf "$f" /usr/bin; done &&\
                                      ln -sf clang /usr/bin/cc && ln -sf clang /usr/bin/c89 &&  ln -sf clang /usr/bin/c99 && ln -sf clang++ /usr/bin/c++ && ln -sf clang++ /usr/bin/g++ && rm -rf /var/lib/apt/lists/* &&\
                                      wget https://archives.boost.io/release/1.82.0/source/boost_1_82_0.tar.gz &&\
                                      tar -xf boost_1_82_0.tar.gz &&\
                                      cd boost_1_82_0 &&\
                                      ./bootstrap.sh --with-libraries="thread" --prefix=/usr &&\
                                      ./b2 link=static -a &&\
                                      ./b2 install &&\
                                      cd .. &&\
                                      git clone -b 20240722.0 https://github.com/abseil/abseil-cpp.git &&\
                                      cmake -B ./abseil-build -S abseil-cpp -GNinja -DCMAKE_INSTALL_PREFIX=./abseil-install -DABSL_PROPAGATE_CXX_STD=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_STANDARD=20 &&\
                                      cmake --build ./abseil-build --target install -- -j6 &&\
                                      git clone -b v29.0-rc1 --depth=1 https://github.com/protocolbuffers/protobuf.git &&\
                                      cd protobuf && git submodule update --init --recursive && cd .. &&\
                                      cmake -B ./protobuf-build -S protobuf -GNinja -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_RPEFIX_PATH=./abseil-install -Dprotobuf_LOCAL_DEPENDENCIES_ONLY=ON -DABSL_PROPAGATE_CXX_STD=ON -Dprotobuf_WITH_ZLIB=ON &&\
                                      cmake --build ./protobuf-build --target install -- -j6

