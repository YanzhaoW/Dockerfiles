ARG BUILD_REPO=fedora
FROM ${BUILD_REPO}:latest

ARG THREAD_NUM=4
ARG FAIRSOFT_VERSION="v1.0.0"
ARG ROOT_VERSION="6.36.00"

ENV SIMPATH="/opt/FairSoft"
ENV FAIRROOTPATH="/opt/FairRoot"
ENV UCESB_DIR="/opt/ucesb"

RUN yes | dnf upgrade --refresh &&\
        echo "strict=False" >> /etc/dnf/dnf.conf &&\
        echo "skip_if_unavailable=True" >> /etc/dnf/dnf.conf &&\
        dnf install -y util-linux which gcc gcc-gfortran clang libcxx-devel langpacks-en net-tools iputils tmux procps cmake ninja-build ccache neovim zsh htop git gdb lldb valgrind wget nc man pmix-devel pmix-tools byacc gawk json-devel graphviz doxygen ghostscript openssh-server xauth byacc conda clang-tools-extra lsb_release fastfetch&&\
        dnf autoremove -y &&\
        mkdir -p /tmp/download && cd /tmp/download &&\
        wget https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.6.tar.gz &&\
        tar -xvzf openmpi-5.0.6.tar.gz && cd openmpi-5.0.6 &&\
        ./configure --with-slurm --with-pmix && make -j${THREAD_NUM} && make install && cd .. &&\
        rm -rf /tmp/download/* &&\
        export CC=clang && export FC=gfortran && export CXX=clang++ &&\
        # install fairsoft &&\
        mkdir -p ${SIMPATH} &&\
        cd /tmp/download &&\
        git clone --depth 1 --branch ${FAIRSOFT_VERSION} https://github.com/YanzhaoW/FairSoft.git &&\
        cd FairSoft && source legacy/setup-fedora.sh &&\
        mkdir build && cd build &&\
        cmake -DCMAKE_INSTALL_PREFIX=${SIMPATH} -DUSE_ROOT_VERSION=${ROOT_VERSION} -GNinja .. && ninja -j${THREAD_NUM} &&\
        rm -rf /tmp/download/* &&\
        # install fairroot &&\
        mkdir -p ${FAIRROOTPATH} &&\
        cd /tmp/download &&\
        git clone --depth 1 --branch dev https://github.com/FairRootGroup/FairRoot.git &&\
        mkdir -p FairRoot/build && cd FairRoot/build && cmake -GNinja -DCMAKE_INSTALL_PREFIX=${FAIRROOTPATH} .. &&\
        ninja -j${THREAD_NUM} && ninja install &&\
        rm -rf /tmp/download/* &&\
        # install ucesb &&\
        make -p /opt && cd /opt &&\
        git clone https://git.chalmers.se/expsubphys/ucesb.git && cd ucesb && make empty/empty &&\
        # remove source files &&\
        cd / && rm -rf /tmp/download && dnf autoremove -y &&\
        # setup root account &&\
        echo "root:root" | chpasswd &&\
        # setup sshd server &&\
        echo "PermitRootLogin yes" >> /etc/ssh/sshd_config &&\
        # setup environments for ssh login: &&\
        touch /etc/profile.d/r3b.sh &&\
        echo "export SIMPATH=${SIMPATH}" >> /etc/profile.d/r3b.sh &&\
        echo "export FAIRROOTPATH=${FAIRROOTPATH}" >> /etc/profile.d/r3b.sh &&\
        echo "export UCESB_DIR=${UCESB_DIR}" >> /etc/profile.d/r3b.sh

CMD ["/usr/sbin/sshd", "-D"]
