# Basic Arch Linux ARM Docker images [![Build Status](https://travis-ci.org/agners/archlinuxarm-docker.svg?branch=master)](https://travis-ci.org/agners/archlinuxarm-docker)

Docker images for Arch Linux ARM on AArch32 (ARMv7-A) and AArch64 (ARMv8-A). Built using native pacman and Docker multi-stage builds. Builds weekly by Travis CI on publicly visible infrastructure using Qemu emulation.

## Running the images

The images are on [Docker Hub](https://hub.docker.com/u/agners/). Use the convenient `docker run`:

    docker run --rm -ti agners/archlinuxarm

Instead of using the multi-arch container above, you can also get the architecture specific image directly:

    docker run --rm -ti agners/archlinuxarm-arm32v7

## Tags

|  Tag   |   Update   |  Type   |                                 Description                                        |
|:------:|:----------:|:-------:|:-----------------------------------------------------------------------------------|
| latest | **weekly** | minimal | minimal Arch Linux ARM with pacman support                                        |

### Layer structure

The image is generated from a freshly built pacman rootfs. Pacman has configured
to delete man pages and clean the package cache after installation to keep
images small.

## Issues and improvements

If you want to contribute, get to the [issues-section of this repository](https://github.com/agners/archlinuxarm-docker/issues).

## Common hurdles

### Setting the timezone

Simply add the `TZ` environment-variable and define it with a valid timezone-value.

```
docker run -e TZ=Europe/Berlin agners/archlinuxarm
```

## Building it yourself

### Prerequisites

- Docker with experimental mode on (required for squash)
- sudo or root is neccessary to setup binfmt for Qemu user mode emulation

### Building

- Prepare binfmt use with Qemu user mode using `sudo ./prepare-qemu`
- Run `BUILD_ARCH=<arch> ./build` to build
  - Use `BUILD_ARCH=arm32v7` for ARMv7 Aarch32
  - Use `BUILD_ARCH=arm64v8` for ARMv8 Aarch64

If you want to push the images, run `./push`. *But be aware you have no push access to the repos! Edit the scripts to push to custom Docker Hub locations!*

### Building from scratch

Since the image depends on itself, the question which arise is how this all
started. The initial containers have been created using the tarballs provided by
the Arch Linux ARM project. I used the following steps to bootstrap for each
architecture:

```
sudo tar xvzf ArchLinuxARM-armv7-latest.tar.gz -C tmp-arch
sudo tar cf ArchLinuxARM-armv7-latest.tar -C tmp-arch/ .
docker import ArchLinuxARM-armv7-latest.tar agners/armv7-archlinux:latest
```

## Credits

Ideas have been taken from already existing Docker files for Arch Linux.
However, this repository takes a slightly different approach to create images.

- https://github.com/archlinux/archlinux-docker
  - Focus on Arch Linux for x86
  - Uses docker run in priviledged mode to build images
- https://github.com/lopsided98/archlinux
  - Uses prebuilt tarballs which contain packages not required in containers
