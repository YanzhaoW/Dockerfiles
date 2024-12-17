FROM fedora:41

ENV CMAKE_GENERATOR=Ninja
ENV CC=clang
ENV CXX="clang++"

RUN yes | dnf update && yes | dnf upgrade &&\
        dnf install -y cmake clang tar git tbb-devel wget ninja root gh zlib-devel
