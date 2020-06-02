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

## Initial setup

```
sudo lxd-image-server init
sudo chown -R lxdadm:www-data /var/www/simplestreams
```

Also make sure the lxdadm user has a password set, so we can upload authorized SSH keys later:

```
sudo passwd lxdadm
```

Finally make sure `openssh-server` is installed, since clients are going to use `scp` to transfer the image files to the LXD Image Server:

```
sudo apt install openssh-server
```

## Troubleshooting

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