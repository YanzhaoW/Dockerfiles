FROM fedora:latest

ARG THREAD_NUM=4
ARG FAIRSOFT_VERSION="v1.0.0"

RUN yes | dnf upgrade --refresh &&\
        echo "strict=False" >> /etc/dnf/dnf.conf &&\
        echo "skip_if_unavailable=True" >> /etc/dnf/dnf.conf &&\
        dnf install -y util-linux which gcc gcc-gfortran clang langpacks-en net-tools iputils tmux procps cmake ninja-build ccache neovim zsh htop git gdb lldb valgrind wget nc man pmix-devel pmix-tools byacc gawk json-devel graphviz doxygen ghostscript openssh-server xauth&&\
        dnf autoremove -y &&\
        mkdir -p /tmp/download && cd /tmp/download &&\
        wget https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.6.tar.gz &&\
        tar -xvzf openmpi-5.0.6.tar.gz && cd openmpi-5.0.6 &&\
        ./configure --with-slurm --with-pmix && make -j${THREAD_NUM} && make install && cd .. &&\
        git clone --depth 1 --branch ${FAIRSOFT_VERSION} https://github.com/YanzhaoW/FairSoft.git &&\
        cd FairSoft && source legacy/setup-fedora.sh &&\
        export CC=clang && export FC=gfortran && export CXX=clang++ &&\
        mkdir build && cd build &&\
        cmake -GNinja .. && ninja -j${THREAD_NUM} &&\
        cd / && rm -rf /tmp/download && dnf autoremove -y

ENV SIMPATH="/usr/local"
