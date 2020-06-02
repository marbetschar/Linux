# LXD Image Server

![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install [LXD Image Server](https://github.com/Avature/lxd-image-server) on Ubuntu 18.04 LTS.

## Prerequisites

- Vanilla Ubuntu 18.04 LTS

## Building the Package

```
sudo apt install git debhelper dh-exec python3 python-dev dh-virtualenv python-pip tox
git clone https://github.com/Avature/lxd-image-server.git
cd lxd-image-server
dpkg-buildpackage -us -uc -b
cd ..
```

## Installation

```
sudo apt install ./lxd-image-server_0.0.1_all.deb
```

## Setup

```
sudo lxd-image-server init
sudo chown -R lxdadm:www-data /var/www/simplestreams
```

Also make sure the lxdadm user has a password set, so we can upload new SSH keys later:

```
sudo passwd lxdadm
```

Finally make sure `openssh-server` is installed, since clients are going to use `scp` to transfer the image files to the LXD Image Server:

```
sudo apt install openssh-server
```

### Troubleshooting

> This system supports the C.UTF-8 locale which is recommended. You might be able to resolve your issue by exporting the following environment variables:
>
>    export LC_ALL=C.UTF-8
>    export LANG=C.UTF-8

Add the file `/etc/profile.d/99-lxd-image-server-locale.sh` with the following content:

```
export LC_ALL=C.UTF-8
export LANG=C.UTF-8
```

Logout and log back in, then try again: `sudo lxd-image-server init`.

## Upload Images to LXD Image Server

### Client Setup

**On the client machine which should upload the LXD image**, make sure we can connect to the LXD Image Server via `ssh`.
For this to work, we first need to create a new SSH key and upload it to our LXD Image Server:

```
ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa.pub lxdadm@lxd.image.server.ip
```

And then test the connection from the client machine:

```
ssh -i ~/.ssh/id_rsa.pub lxdadm@lxd.image.server.ip
```

### Upload Images

We assume you created an LXD Image using [distrobuilder](https://github.com/lxc/distrobuilder) and therefore have two files ready to upload:

- lxd.tar.xz
- rootfs.squashfs

To successfully publish an image through LXD Image Server, you need to generate it using `distrobuilder` and then upload it with `scp`:

```
#!/bin/bash
set -e

LXD_IMAGE_SERVER_HOST=lxd.image.server.ip
LXD_IMAGE_SERVER_USER=lxdadm

LXD_IMAGE_OS=ubuntu
LXD_IMAGE_RELEASE=focal
LXD_IMAGE_ARCH=amd64
LXD_IMAGE_VARIANT=xfce
LXD_IMAGE_VERSION=$(date '+%Y%m%d_%H:%m')
LXD_IMAGE_PATH="$LXD_IMAGE_OS/$LXD_IMAGE_RELEASE/$LXD_IMAGE_ARCH/$LXD_IMAGE_VARIANT/$LXD_IMAGE_VERSION"

cd /tmp
mkdir -p "$LXD_IMAGE_PATH"

cd "$LXD_IMAGE_PATH"
sudo ~/go/bin/distrobuilder build-lxd /lxc-ci/images/"$LXD_IMAGE_OS".yaml \
    -o image.release="$LXD_IMAGE_RELEASE" \
    -o image.architecture="$LXD_IMAGE_ARCH" \
    -o image.variant="$LXD_IMAGE_VARIANT" \
    -o source.url="http://us.archive.ubuntu.com/ubuntu"
cd /tmp

scp -r "$LXD_IMAGE_PATH/lxd.tar.gz" $LXD_IMAGE_SERVER_USER@$LXD_IMAGE_SERVER_HOST:/var/www/simplestreams/images/
scp -r "$LXD_IMAGE_PATH/rootfs.squashfs" $LXD_IMAGE_SERVER_USER@$LXD_IMAGE_SERVER_HOST:/var/www/simplestreams/images/

rm -rf "$LXD_IMAGE_OS"
```