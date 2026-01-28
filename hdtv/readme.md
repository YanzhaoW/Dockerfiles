### MacOS

1. Install docker daemon (see the instruction [here](https://docs.docker.com/desktop/setup/install/mac-install/)).

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
    docker run -it --rm -e DISPLAY=docker.for.mac.host.internal:0 -v $(pwd):/data --name htest yanzhaowang/hdtv:fedora-arm
    ```

#### Additional references

* [X11 forwarding on macOS and docker](https://gist.github.com/sorny/969fe55d85c9b0035b0109a31cbcb088)

### Inside a linux server via ssh

First, please make sure `ssh -Y` is used when logging to the server. Once inside the server, run

```bash
docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --entrypoint "/bin/bash" --name htest hdtv:latest
```
