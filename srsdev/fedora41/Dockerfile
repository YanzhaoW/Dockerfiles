FROM fedora:41

ENV CMAKE_GENERATOR=Ninja
ENV CC=clang
ENV CXX="clang++"

run yes | dnf udpate && yes | dnf upgrade &&\
        dnf install -y clang protobuf boost git tbb wget boost-devel tbb-devel protobuf-devel ninja

