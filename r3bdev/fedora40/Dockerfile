FROM fedora:40

RUN yes | dnf upgrade --refresh &&\
        dnf install -y util-linux which gcc clang langpacks-en net-tools iputils tmux procps cmake ninja-build ccache neovim zsh htop git gdb lldb valgrind wget nc man

RUN mkdir -p /opt/FairSoft/jan24p2 && mkdir -p /tmp/download && cd /tmp/download &&\
        wget https://github.com/FairRootGroup/FairSoft/archive/refs/tags/jan24p2.tar.gz &&\
        tar -xf jan24p2.tar.gz && cd FairSoft-jan24p2 &&\
        source legacy/setup-fedora.sh &&\
        mkdir build && cd build &&\
        cmake -DCMAKE_INSTALL_PREFIX=/opt/FairSoft/jan24p2 -GNinja .. && ninja -j4 &&\
        cd / && rm -rf /tmp/download

ENV SIMPATH="/opt/FairSoft/jan24p2"
