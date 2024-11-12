FROM ubuntu:22.04

RUN apt-get update &&\ cd / &&\
        sudo apt install -y binutils cmake dpkg-dev g++ gcc libssl-dev git libx11-dev \
        libxext-dev libxft-dev libxpm-dev python3 libtbb-dev libgif-dev wget zlib1g zlib1g-dev ninja-build&&\
        git clone -b llvmorg-19.1.3 --depth=1 https://github.com/llvm/llvm-project.git &&\
        mkdir llvm-build && cmake -DLLVM_ENABLE_PROJECTS=clang -DCMAKE_INSTALL_PREFIX=/usr -DLLVM_ENABLE_RUNTIMES="libcxx;libcxxabi" -DCMAKE_BUILD_TYPE=Release -GNinja -S llvm-project/llvm -B llvm-build &&\
        cmake --build llvm-build --target install -- -j4
