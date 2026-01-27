### Inside a linux server via ssh

First, please make sure `ssh -Y` is used when logging to the server. Once inside the server, run

```bash
docker run --env "DISPLAY" -v ${HOME}/.Xauthority:/root/.Xauthority -v $(pwd):/data -it --rm --net=host --entrypoint "/bin/bash" --name htest hdtv:latest
```
