### Build cvmfs environment for R3BRoot CI

Note that in the dockerfile for debian11 and debian 12, the cvmfs is built from the source because of [this issue](https://github.com/cvmfs/cvmfs/issues/3747). Once the devel branch is pushed to the pre-built binary distribution source, cmvfs can be more easily installed with `apt-get`, as is instructed in [its website](https://cvmfs.readthedocs.io/en/stable/cpt-quickstart.html#building-from-source):

```bash
wget https://cvmrepo.s3.cern.ch/cvmrepo/apt/cvmfs-release-latest_all.deb
sudo dpkg -i cvmfs-release-latest_all.deb
rm -f cvmfs-release-latest_all.deb
sudo apt-get -y update
sudo apt-get -y install cvmfs
```

Please remove the following commands if the cvmfs is pulled from the pre-built binary:

```bash
    git clone --depth 1 --branch devel https://github.com/cvmfs/cvmfs.git && cd cvmfs &&\
    mkdir build && cd build && cmake -GNinja .. &&\
    ninja -j${NThreads} && ninja install && cd /misc && rm -rf cvmfs &&\
```

The built images are hosted in Dockerhub. Please visit [my repository](https://hub.docker.com/r/yanzhaowang/cvmfs_clang/tags) to pull the images.
