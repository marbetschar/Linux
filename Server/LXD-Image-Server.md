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
sudo apt install ./lxd-image-server_0.0.1_all_deb
```

## Setup

```
sudo lxd-image-server init
sudo chown -R lxdadm:www-data /var/www/simplestreams
```

Once everythign is in place, we need to generate the SSH key for the lxdadm user to allow uploading images:

```
sudo su --login lxdadm
ssh-keygen
```

## Upload Images to LXD Image Server

### Client Setup

**On the LXD Image Server**, make sure `openssh-server` is installed, since we are going to use `scp` to transfer the image files to the LXD Image Server:

```
sudo apt install openssh-server
sudo systemctl enable openssh-server
sudo systemctl start openssh-server
```

Also, copy the SSH public key to a public location temporarily, so we can easily download it from the client machine:

```
sudo su --login lxdadm
cp -p ~/.ssh/id_rsa.pub /var/www/simplestreams/id_rsa.pub
```

**On the client machine which should upload the LXD image**, make sure we can connect to the LXD Image Server via `ssh`.
For this to work, we need to download the previously created public key:

```
wget -O ~/.ssh/lxd-image-server.pub https://lxd.image.server.ip:8443/id_rsa.pub --no-check-certificate
```

And then test the connection from the client machine:

```
ssh -i ~/.ssh/lxd-image-server.pub lxdadm@lxd.image.server.ip
```

If everything works well, **don't forget to delete the public key** on the LXD Image Server at `/var/www/simplestreams/id_rsa.pub`.

### Upload Images

We assume you created an LXD Image using [distrobuilder](https://github.com/lxc/distrobuilder) and therefore have two files ready to upload:

- lxd.tar.xz
- rootfs.squashfs

To successfully publish them through LXD Image Server, you need to upload them using `scp`:

```
#!/bin/bash
set -e

LXD_IMAGE_OS=ubuntu
LXD_IMAGE_RELEASE=focal
LXD_IMAGE_ARCH=amd64
LXD_IMAGE_VARIANT=xfce
LXD_IMAGE_VERSION=$(date '+%Y%m%d_%H:%m')
LXD_IMAGE_PATH="$LXD_IMAGE_OS/$LXD_IMAGE_RELEASE/$LXD_IMAGE_ARCH/$LXD_IMAGE_VARIANT/$LXD_IMAGE_VERSION"

mkdir -p "$LXD_IMAGE_PATH"
mv lxd.tar.gz "$LXD_IMAGE_PATH"
mv rootfs.squashfs "$LXD_IMAGE_PATH"

scp -i ~/.ssh/lxd-image-server.pub -r "$LXD_IMAGE_PATH/lxd.tar.gz" lxdadm@lxd.image.server.ip
scp -i ~/.ssh/lxd-image-server.pub -r "$LXD_IMAGE_PATH/rootfs.squashfs" lxdadm@lxd.image.server.ip

rm -rf "$LXD_IMAGE_OS"
```