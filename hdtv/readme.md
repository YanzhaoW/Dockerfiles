## Run hdtv program via Docker container

Following systems are supported:

- [Linux](#linux)
- [MacOS](#macos)
- [Windows11](#windows-11)
- [Linux server](#linux-server)
  - [With sudo privilege](#with-sudo-privilege)
  - [Without sudo privilege (Podman)](#without-sudo-privilege-podman)

General notice:

1. `docker` command in a Linux system, by default, requires the sudo permission. Please see [this instruction](https://docs.docker.com/engine/install/linux-postinstall/) to enable sudo permission by default when using `docker`.

2. There are two images available from Dockerhub: `yanzhaowang/hdtv:fedora` and `yanzhaowang/hdtv:fedora_arm`. The first one is for x86_64 machines, which is used by servers and most of the Windows PC. The second one is for machines with ARM architecture, which is used by MacBooks with Apple Silicon and some Windows laptops.

3. The command to run the docker container is pretty long. Thus, it's recommended to create an alias to the command in `.bashrc` (bash), `.zshrc` (zsh). In Windows 11, check this [instruction](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases) to create a permanent alias.

4. The `docker run` command below mounts your current folder to the `/data` inside the container. If another folder needs to be mounted, add `-v host_file_locaiton/container_location` to the command.

### Linux

1. Make sure the docker engine has been installed (For the installation, please check the [instruction here](https://docs.docker.com/engine/install)).

2. Make sure the hdtv image has been downloaded/updated. For the download or update, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Make sure xhost permission for the host is enabled. To enable it, run:

   ```bash
   xhost +
   ```

4. Run the docker container with:

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

### MacOS

1. Make sure the docker daemon has been installed. For the installation, see the instruction [here](https://docs.docker.com/desktop/setup/install/mac-install/).

2. Make sure the latest version (including beta version) of XQuartz has been installed. It can be installed from its [official website](https://www.xquartz.org/releases/index.html). To check whether XQuartz is working, run:

   ```bash
   xclock
   ```

   A clock UI should appear.

3. Make sure the hdtv image has been downloaded/updated. To download or update it, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora-arm
   ```

4. Make sure xhost permission for the host is enabled. To enable it, run:

   ```bash
   xhost +
   ```

5. Run the docker container with:

   ```bash
   docker run -it --rm -e DISPLAY=docker.for.mac.host.internal:0 -v $(pwd):/data --name hdtv yanzhaowang/hdtv:fedora-arm
   ```

#### Additional references

- [X11 forwarding on macOS and docker](https://gist.github.com/sorny/969fe55d85c9b0035b0109a31cbcb088)

### Windows 11

1. Make sure the docker desktop for Windows has been installed. To install it, please check [here](https://docs.docker.com/desktop/setup/install/windows-install/).

2. Make sure XMing has been installed. To install it, please check [here](https://sourceforge.net/projects/xming/).

3. Make sure the docker image has been downloaded and updated. To download or update it, open Windows 11 Powershell (MobaXTerm can be used for older Windows) and run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

4. Go to the folder which contains the data via File Explorer, right click and choose "Open in Terminal".
5. Run the docker container with (make sure XMing is still running in the background):

   ```bash
   docker run -it --rm -e DISPLAY=host.docker.internal:0 -v "$((Get-Location).Path):/data" --name hdtv yanzhaowang/hdtv:fedora-arm
   ```

### Linux server

Please make sure `ssh -Y` is used when logging into the server.

```bash
ssh -Y username@server_name
```

#### With sudo privilege

1. Make sure docker engine is installed. Check the [section above](#linux) for its installation.

2. Make sure the hdtv image has been downloaded/updated. To download or update it, run:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Once inside the server, run

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

#### Without sudo privilege (Podman)

1. Make sure Podman is installed in the system.

2. Make sure the hdtv image has been downloaded/updated. To download or update it, run:

   ```bash
   podman pull docker.io/yanzhaowang/hdtv:fedora
   ```

3. Run the docker container:

   ```bash
   podman run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority:rw -v $(pwd):/data --security-opt label=type:container_runtime_t -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

   NOTE: `--security-opt label=type:container_runtime_t` is not needed if the server system doesn't have SELinux enabled.
