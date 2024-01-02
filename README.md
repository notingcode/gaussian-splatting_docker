# Docker Build and Running Built Container

## How to start build

If **Docker Desktop** is installed, make sure the image is built with `sudo` privilege. If `sudo` privilege is not used, the image will not be visible to the local docker engine.

```[bash]
# Make sure you have 'nvidia-container-toolkit' installed on your host computer
sudo docker build --build-arg user=${USER} -t splat:base .
```

- Check Dockerfile for build details.

[Install NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## Docker run container (Linux and WSL)

```[bash]
sudo docker run -it --gpus all splat:base
```

COLMAP and Gaussian splat pipeline available via CLI.

## How to start GUI container with built image on Linux

### X Server Forwarding Prerequisite

Install [x11docker](https://github.com/mviereck/x11docker) (On Linux)

```[bash]
curl -fsSL https://raw.githubusercontent.com/mviereck/x11docker/master/x11docker | sudo bash -s -- --update
```

### Run the container instance

```[bash]
sudo x11docker -i --sudouser --gpu --runtime=nvidia --xwayland splat:base
```

Please refer to [x11docker manual](https://github.com/mviereck/x11docker?tab=readme-ov-file#security) for any user permission problems inside the container.

### Sudo Privileges (Important)

Conda is only available with root user. Set user to `root` when attached to a running container.

```[bash]
# Password is always x11docker
username@9ceb007da3a5:~$ su
Password: x11docker
```

### Sharing files or folders from Host

[x11docker manual](https://github.com/mviereck/x11docker?tab=readme-ov-file#shared-folders-volumes-and-home-in-container)

Check the example below.

```[bash]
# Path must be absolute or it can be a VOLUME
sudo x11docker -i --sudouser --gpu --runtime=nvidia --xwayland --share /home/${USER}/train_dataset/example splat:base
```

The contents in the shared path can be accessed in the container using the same path.

You can also use [`docker cp`](https://docs.docker.com/engine/reference/commandline/cp/) with the running container.

## SIBR Viewer Unsolved Error

- When launching SIBR viewer, OpenGL/CUDA interop error occurs that forces scene rendering to be done on the CPU. The solution to this problem is still not found.
