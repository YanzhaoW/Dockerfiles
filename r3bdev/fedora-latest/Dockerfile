FROM fedora:latest

RUN yes | dnf upgrade --refresh &&\
        dnf install -y util-linux which gcc clang langpacks-en net-tools iputils tmux procps cmake ninja-build ccache neovim zsh htop git gdb lldb valgrind wget nc man pmix-devel pmix-tools byacc &&\
        mkdir -p /tmp/download && cd /tmp/download &&\
        wget https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.6.tar.gz &&\
        tar -xvzf openmpi-5.0.6.tar.gz && cd openmpi-5.0.6 &&\
        ./configure --with-slurm --with-pmix && make -j6 && make install && cd .. &&\
        git clone --depth 1 --branch jan24p4 https://github.com/FairRootGroup/FairSoft.git &&\
        cd FairSoft && source legacy/setup-fedora.sh &&\
        mkdir build && cd build &&\
        cmake -GNinja .. && ninja -j6 &&\
        cd / && rm -rf /tmp/download

ENV SIMPATH="/usr/local"
