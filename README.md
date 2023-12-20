# Docker Build and Running Built Container

## How to start build

If **Docker Desktop** is installed, make sure the image is built with `sudo` privilege. If `sudo` privilege is not used, the image will not be visible to the local docker engine.

```[bash]
# Make sure you have 'nvidia-container-toolkit' installed on your host computer
sudo docker build -t splat:base .
```

- Check Dockerfile for build info.

[Install NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## X Server Forwarding Prerequisite

### Install [x11docker](https://github.com/mviereck/x11docker) (Linux and Windows Subsystem for Linux)

```[bash]
curl -fsSL https://raw.githubusercontent.com/mviereck/x11docker/master/x11docker | sudo bash -s -- --update
```

## How to start container with built image

```[bash]
sudo x11docker -i --sudouser --gpu --runtime=nvidia --xwayland splat:base
```

Please refer to [x11docker manual](https://github.com/mviereck/x11docker?tab=readme-ov-file#security) for any user permission problems inside the container.

### Sharing files or folders from Host

[x11docker manual](https://github.com/mviereck/x11docker?tab=readme-ov-file#shared-folders-volumes-and-home-in-container)

```[bash]
# Example
# Path must be absolute or it can be a VOLUME
sudo x11docker -i --sudouser --gpu --runtime=nvidia --xwayland --share /home/${USER}/train_dataset/example splat:base
```

The contents in the shared path can be accessed in the container using the same path.

## SIBR Viewer Unsolved Error

- When launching SIBR viewer, OpenGL/CUDA interop error occurs that forces rendering to be done on the CPU. The solution to this problem is still not found.
