# LXD Desktop CI

![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install [LXD Desktop CI](https://github.com/desktop-lxc/lxc-ci) to generate desktop images using [distrobuilder](https://github.com/lxc/distrobuilder) on Ubuntu 18.04 LTS.

## Prerequisites

- [LXD Image Server](LXD-Image-Server.md)
- Vanilla Ubuntu 18.04 LTS

## Install CI Scripts

Add a new user to execute CI commands with:

```
sudo useradd --create-home --groups sudo,lxd lxdadm
```

Install git:

```
sudo apt install git
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

Make sure to add your user to the `lxdadm` group, to avoid permission issues:

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

Configure weekly execution of the `update-image` job using `cron.weekly`:

```
sudo ln -s /lxc-ci/desktop-lxc/update-images /etc/cron.weekly/desktop-lxc-update-images
```

Next, configure the SSH connection to be used to upload the images:

**On the client machine which should upload the LXD image**, make sure we can connect to the LXD Image Server via `ssh`.
For this to work, we first need to create a new SSH key and upload it to our LXD Image Server:

```
sudo su --login lxdadm
ssh-keygen
ssh-copy-id lxdadm@lxd-image-server

# test the connection from the client machine:
ssh lxdadm@lxd-image-server

# logout as lxdadm if everything is fine:
exit
```

## Install distrobuilder

Install the required dependencies:

```
sudo apt install golang-go debootstrap rsync gpg squashfs-tools make git
```

```
sudo su --login lxdadm
go get -d -v github.com/lxc/distrobuilder/distrobuilder
cd ~/go/src/github.com/lxc/distrobuilder
make
exit
```

Allow `sudo` without password for `lxdadm`:

```
sudo visudo
+ lxdadm    ALL=(ALL) NOPASSWD:ALL
```

## Test Everything

Make sure everything is set up correctly by manually executing the cron job (this runs quite long):

```
sudo /lxc-ci/desktop-lxc/update-image ubuntu focal amd64 xfce &
sudo tail -f /var/log/lxc-ci/ubuntu-focal-amd64-xfce.out
```
