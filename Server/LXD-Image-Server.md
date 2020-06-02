# LXD Image Server

![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install [LXD Image Server](https://github.com/Avature/lxd-image-server) on Ubuntu 18.04 LTS.

## Prerequisites

- Vanilla Ubuntu 18.04 LTS

## Building the Package

```
git clone https://github.com/Avature/lxd-image-server.git
cd lxd-image-server
sudo apt install debhelper dh-exec python3 python-dev dh-virtualenv python-pip tox
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

**On the LXD Image Server**, make sure `openssh-server` is installed, since we are going to use `scp` to transfer the image files:

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


To upload images:

https://github.com/Avature/lxd-image-server/issues/1



