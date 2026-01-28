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

2. There are two images available from dockerhub: `yanzhaowang/hdtv:fedora` and `yanzhaowang/hdtv:fedora_arm`. The first one is for x86_64 machines, which is used by servers and most of the Windows PC. The second one is for machines with ARM architecture, which is used by Macbooks with Apple silicon and some Windows laptops.

3. The command to run the docker container is pretty long. Thus it's recommended to create an alias to the command in `.bashrc` (bash), `.zshrc` (zsh). In Windows 11, check this [instruction](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases) to create a permanent alias.

4. The `docker run` command below mount your current folder to the `/data` inside the container. If another folder needs to be mounted, add `-v host_file_locaiton/container_location` to the command.

### Linux

1. Install the docker engine (see the [instruction](https://docs.docker.com/engine/install)).

2. Download the hdtv image:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Enable xhost permission for the host:

   ```bash
   xhost +
   ```

   You need to make sure this has been run before running the container in the next step.

4. Run the docker container with:

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

### MacOS

1. Install the docker daemon (see the instruction [here](https://docs.docker.com/desktop/setup/install/mac-install/)).

2. Download the hdtv image:

   ```bash
   docker pull yanzhaowang/hdtv:fedora-arm
   ```

3. Install the latest version (including beta version) of XQuartz from its [official website](https://www.xquartz.org/releases/index.html). To check whether XQuartz is working, run:

   ```bash
   xclock
   ```

   A clock UI should appear.

4. Enable xhost permission for the host:

   ```bash
   xhost +
   ```

   You need to make sure this has been run before running the container in the next step.

5. Run the docker container with:

   ```bash
   docker run -it --rm -e DISPLAY=docker.for.mac.host.internal:0 -v $(pwd):/data --name hdtv yanzhaowang/hdtv:fedora-arm
   ```

#### Additional references

- [X11 forwarding on macOS and docker](https://gist.github.com/sorny/969fe55d85c9b0035b0109a31cbcb088)

### Windows 11

1. Install docker desktop for Windows [here](https://docs.docker.com/desktop/setup/install/windows-install/).

2. Install XMing [here](https://sourceforge.net/projects/xming/).

3. Once the docker desktop is installed, open Windows 11 Powershell (MobaXTerm can be used for older Windows) and install the image:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

4. Go to the folder which contains the data via File Explorer , right click and choose "Open in Terminal".
5. Run the docker container with (make sure XMing is running in the background):

   ```bash
   docker run -it --rm -e DISPLAY=host.docker.internal:0 -v "$((Get-Location).Path):/data" --name hdtv yanzhaowang/hdtv:fedora-arm
   ```

### Linux server

Please make sure `ssh -Y` is used when logging to the server.

```bash
ssh -Y username@server_name
```

#### With sudo privilege

1. Install docker engine (see the [section above](#linux)).

2. Pull the docker image:

   ```bash
   docker pull yanzhaowang/hdtv:fedora
   ```

3. Once inside the server, run

   ```bash
   docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

#### Without sudo privilege (Podman)

1. Make sure Podman is installed in the system.

2. Pull the image from dockehub:

   ```bash
   docker pull docker.io/yanzhaowang/hdtv:fedora
   ```

3. Run the docker container:

   ```bash
   podman run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority:rw -v $(pwd):/data --security-opt label=type:container_runtime_t -it --rm --net=host --name hdtv yanzhaowang/hdtv:fedora
   ```

   NOTE: `--security-opt label=type:container_runtime_t` is not needed if the server system doesn't have SELinux enabled.
