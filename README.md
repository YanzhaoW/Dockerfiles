### Steps to push to Dockerhub

1. Build the image
```bash
docker build -t [local_image:local_tag] [path_to_folder]
```
where `[path_to_folder]` is the folder containing the Dockerfile.

2. Tag the image
```bash
docker tag [local_image:local_tag] [Dockerhub_repo:remote_tag]
```
e.g. `[Dockerhub_repo:remote_tag]` could be `yanzhaowang/r3bdev`

3. Push the image
```bash
docker push [Dockerhub_repo:remote_tag]
```
