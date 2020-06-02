# LXD Desktop CI

![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install [LXD Desktop CI](https://github.com/desktop-lxc/lxc-ci) to generate desktop images using [distrobuilder](https://github.com/lxc/distrobuilder) on Ubuntu 18.04 LTS.

## Prerequisites

- [LXD Image Server](LXD-Image-Server.md)
- Vanilla Ubuntu 18.04 LTS

## Basic Setup

Add a new user to execute CI commands with:

```
useradd -G sudo,lxd lxdadm
```

Install the required dependencies:

```
sudo apt install golang-go debootstrap rsync gpg squashfs-tools make git
```

Configure git to use your name and email address:

```
git config --global user.name "Mona Lisa"
git config --global user.email "youremail@yourdomain.com"
```

Generate a new SSH key and [add it to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account):

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cat ~/.ssh/id_rsa.pub
```

```
git clone git@github.com:desktop-lxc/lxc-ci.git
sudo chown -R lxdadm:lxdadm lxc-ci
sudo mv lxc-ci /
cd /lxc-ci
```

**IMPORTANT:** In case you cloned the repository with another user than `lxdadm`, make sure to add this user to the `lxdadm` group, to avoid permission issues:

```
# make sure you logout and login after
# adding the lxdadm group to your user:
sudo usermod -a -G lxdadm <username>
```

Make sure the correct branch is checked out:

```
cd /lxc-ci
git checkout desktop-lxc
```

## Install Distrobuilder

```
su lxdadm -
go get -d -v github.com/lxc/distrobuilder/distrobuilder
cd ~/go/src/github.com/lxc/distrobuilder
make
exit
```

