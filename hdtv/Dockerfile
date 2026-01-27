ARG BUILD_REPO=fedora
FROM ${BUILD_REPO}:latest

RUN yes | dnf upgrade --refresh &&\
    dnf install -y gcc root python3-root wget git cmake xorg-x11-server-devel which xclock &&\
    cd /root &&\
    export PYTHONPATH="$(ls -d /usr/lib64/python*/site-packages/ROOT | sed "s/\/ROOT//")" &&\
    wget -O Miniforge3.sh "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh" &&\
    bash Miniforge3.sh -b -p "${HOME}/conda" &&\
    echo "source ${HOME}/conda/etc/profile.d/conda.sh" >> ~/.bashrc &&\
    echo "export PYTHONPATH=${PYTHONPATH}" >> ~/.bashrc &&\
    echo "conda activate hdtv" >> ~/.bashrc &&\
    source "${HOME}/conda/etc/profile.d/conda.sh" &&\
    git clone -b "compile" https://github.com/YanzhaoW/hdtv.git &&\
    cd hdtv &&\
    sed -i "s/- python=.*/- python=$(root-config --python-version)/" ./environment.yml &&\
    conda env create -f environment.yml --solver libmamba &&\
    conda activate hdtv &&\
    ./src/hdtv/app.py --rebuild-usr &&\
    rm -rf /root/Miniforge3.sh

COPY --chmod=0755 entrypoint.sh /root/entrypoint.sh

WORKDIR /data

ENTRYPOINT ["/bin/bash", "-l", "-c", "/root/entrypoint.sh"]
