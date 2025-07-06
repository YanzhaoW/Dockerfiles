ARG BASE_IMAG="fedora:42"

FROM ${BASE_IMAG}

ARG GCC_VERSION="15.1.0"
ARG CLANG_VERSION=20
ARG RELEASE=fedora
ARG THREAD_NUM=4

ENV CMAKE_GENERATOR=Ninja
ENV CC=clang
ENV CXX="clang++"

RUN yes | dnf update && yes | dnf upgrade &&\
        dnf install -y cmake clang tar git tbb-devel wget ninja root gh zlib-devel
