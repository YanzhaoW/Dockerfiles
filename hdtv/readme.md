# How to run hdtv program via Docker container

Following systems are supported:

- [Linux](#linux)
- [MacOS](#macos)
- [Windows11](#windows-11)
- [Linux server](#linux-server)
  - [With sudo privilege](#with-sudo-privilege)
  - [Without sudo privilege (Podman)](#without-sudo-privilege-podman)

General notice:

* The `docker` command in a Linux system, by default, requires the sudo permission. Please check out this [instruction](https://docs.docker.com/engine/install/linux-postinstall/) to enable the sudo permission by default when using `docker`.

* There are two images available at Dockerhub: `yanzhaowang/hdtv:fedora` and `yanzhaowang/hdtv:fedora_arm`. The first one is for machines with the x86\_64 architecture, which is used by servers and most of the Windows PCs. The second one is for machines with the ARM architecture, which is used by MacBooks with Apple Silicon and some Windows/Linux laptops.

* Commands that run docker containers are usually pretty long. Thus, it's recommended to create an alias of the command in `.bashrc` (bash), `.zshrc` (zsh). For Windows 11, check out this [instruction](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases) to create a permanent alias.

* The `docker run` command below mounts your current folder and `~/.local/share/hdtv` to the `/data` and `/root/.local/share/hdtv` inside the container. If another folder needs to be mounted, add `-v host_file_locaiton:container_location` to the command.

* Even though the docker desktop provides some functionalities to run the docker container, the following instructions are done only via terminals (PowerShell in case of Windows).

## Linux

1. Make sure the docker engine has been installed. To install it, please check out this [instruction](https://docs.docker.com/engine/install).

2. Make sure the hdtv image has been downloaded and updated. For the download or update, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Make sure xhost permission for the host is enabled. To enable it, run:

   ```bash
   xhost +
   ```

4. Run the docker container with:

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v ${HOME}/.local/share/hdtv:/root/.local/share/hdtv -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

## MacOS

1. Make sure the docker daemon has been installed. For the installation, check out this instruction [here](https://docs.docker.com/desktop/setup/install/mac-install/).

2. Make sure the latest version (including beta versions) of XQuartz has been installed. It can be installed from its [official website](https://www.xquartz.org/releases/index.html). To check whether XQuartz is working, run:

   ```bash
   xclock
   ```

   A clock UI should appear.

3. Make sure the hdtv image has been downloaded and updated. To download or update it, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora-arm
   ```

4. Make sure xhost permission for the host is enabled. To enable it, run:

   ```bash
   xhost +
   ```

5. Run the docker container with:

   ```bash
   docker run -it --rm -e DISPLAY=docker.for.mac.host.internal:0 -v ${HOME}/.local/share/hdtv:/root/.local/share/hdtv -v $(pwd):/data --name hdtv yanzhaowang/hdtv:fedora-arm
   ```

### Additional references

- [X11 forwarding on macOS and docker](https://gist.github.com/sorny/969fe55d85c9b0035b0109a31cbcb088)

## Windows 11

1. Make sure the docker desktop for Windows has been installed. To install it, please check out [this](https://docs.docker.com/desktop/setup/install/windows-install/).

2. Make sure XMing has been installed. To install it, please check out [this](https://sourceforge.net/projects/xming/).

3. Make sure the docker image has been downloaded and updated. To download or update it, open the Windows 11 PowerShell (MobaXTerm can be used for older Windows versions) and run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

4. Go to the folder which contains the data via File Explorer, right click and choose "Open in Terminal".
5. Run the docker container with (make sure XMing is still running in the background):

   ```bash
   docker run -it --rm -e DISPLAY=host.docker.internal:0 -v $HOME\AppData\Local\hdtv:/root/.local/share/hdtv -v "$((Get-Location).Path):/data" --name hdtv yanzhaowang/hdtv:fedora
   ```

## Linux server

Please make sure `ssh -Y` is used when logging into the server.

```bash
ssh -Y username@server_name
```

### With sudo privilege

1. Make sure the docker engine is installed. Check out the [section above](#linux) for its installation.

2. Make sure the hdtv image has been downloaded/updated. To download or update it, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Once inside the server, run

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v ${HOME}/.local/share/hdtv:/root/.local/share/hdtv -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

### Without sudo privilege (Podman)

1. Make sure Podman is installed on the server.

2. Make sure the hdtv image has been downloaded/updated. To download or update it, run:

   ```bash
   podman pull docker.io/yanzhaowang/hdtv:fedora
   ```

3. Run the docker container:

   ```bash
   podman run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority:rw -v ${HOME}/.local/share/hdtv:/root/.local/share/hdtv -v $(pwd):/data --security-opt label=type:container_runtime_t -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

   NOTE: `--security-opt label=type:container_runtime_t` is not needed if SELinux is not enabled on the server.
