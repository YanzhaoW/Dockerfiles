FROM gcc:14.2.0-bookworm

ENV CMAKE_GENERATOR=Ninja
ENV CC="clang --gcc-toolchain=/usr/local"
ENV CXX="clang++ --gcc-toolchain=/usr/local"


RUN apt-get update &&\ 
apt install -y cmake wget ninja-build git&&\
        echo "deb http://apt.llvm.org/bookworm/ llvm-toolchain-bookworm-19 main" > /etc/apt/sources.list.d/llvm.list &&\
        wget -qO /etc/apt/trusted.gpg.d/llvm.asc https://apt.llvm.org/llvm-snapshot.gpg.key &&\
        apt-get -qq update &&\
        apt-get install -qqy -t llvm-toolchain-bookworm-19 clang-19 &&\
        for f in /usr/lib/llvm-*/bin/*; do ln -sf "$f" /usr/bin; done &&\
                                      ln -sf clang /usr/bin/cc && ln -sf clang /usr/bin/c89 &&  ln -sf clang /usr/bin/c99 && ln -sf clang++ /usr/bin/c++ && ln -sf clang++ /usr/bin/g++ && rm -rf /var/lib/apt/lists/*
