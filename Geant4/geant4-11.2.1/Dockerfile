FROM fedora:39

RUN yes | dnf upgrade --refresh &&\
    dnf install -y which gcc-c++ cmake ccache git python ninja-build expat expat-devel assimp assimp-devel root-6.30.06&&\
    python3 -m ensurepip --upgrade &&\
    python3 -m pip install conan &&\
    cd / && mkdir software && cd software &&\
    git clone https://github.com/Geant4/geant4.git --branch v11.2.1 --depth 1 &&\
    cd geant4 && mkdir build && cmake -GNinja -B build -S . &&\
    cmake --build build -- -j 4 && cmake --install build &&\
    geant4-config --install-datasets &&\
    rm -rf /software

# Run conan config install utils/settings.yml
