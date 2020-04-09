# elementary OS Daily in LXD Container

How to run elementary OS Daily from within an LXD Container for elementary OS Desktop development.

## Table of Contents

- [Install LXD](#install-lxd)
- [Add support for GUI applications to LXD](#add-support-for-gui-applications-to-lxd)
- [Setup daily container](#setup-daily-container)
- [Usage of daily container](Usage-of-Daily-in-LXD-Container.md)
- [Sources](#sources)

## Install LXD

Fire up a Terminal to install and initialize LXD:

```
sudo apt install lxd
sudo lxd init            # use default values for everything
```

## Add support for GUI applications to LXD

### Map the User ID of the host to the container

Run the following on the host (only once). The command appends a new entry in both the _**/etc/subuid**_ and _**/etc/subgid**_ subordinate UID/GID files. It allows the LXD service (that runs as root) to remap our user's ID ($UID, from the host) as requested.

```
echo "root:$UID:1" | sudo tee -a /etc/subuid /etc/subgid
```

### Create the _gui_ LXD profile

FIrst off we create a new profile called **gui** with the following command:

```
sudo lxc profile create gui
```

Then we make sure it is configured correctly by executing the following command:

```
cat << EOF | sudo lxc profile edit gui
config:
  environment.DISPLAY: :0
  raw.idmap: both $UID $UID
  user.user-data: |
    #cloud-config
    runcmd:
      - 'sed -i "s/; enable-shm = yes/enable-shm = no/g" /etc/pulse/client.conf'
      - 'echo export PULSE_SERVER=unix:/tmp/.pulse-native | tee --append /home/ubuntu/.profile'
    packages:
      - x11-apps
      - mesa-utils
      - pulseaudio
description: GUI LXD profile
devices:
  PASocket:
    path: /tmp/.pulse-native
    source: /run/user/$UID/pulse/native
    type: disk
  X0:
    path: /tmp/.X11-unix/X0
    source: /tmp/.X11-unix/X0
    type: disk
  mygpu:
    type: gpu
name: gui
used_by:
EOF
```

Check everything went fine:

```
sudo lxc profile show gui
```

### Allow containers to use the :0 display

We need to allow containers to use the `:0` display of the host machine. In order to do so, add
the following custom command in **System Settings > Applications > Startup**:

```
xhost +local:
```

## Setup daily container

Create a new container named `daily` which is based on Ubuntu LTS 18.04 and assign the `default` and `gui` profiles to it:

```
$ sudo lxc launch --profile default --profile gui ubuntu:18.04 daily
Creating daily
Starting daily
```

### Rename User

_Although this step is completely optional, it is highly recommended because it ensures we'll get a nicely named prompt which avoids potential confusion if you deal with multiple containers._

In a nutshell we rename the existing `ubuntu` user to `elementary`. For this, we need to jump into the newly created `daily` container in order to ensure the `ubuntu` user gets created:

```
$ sudo lxc exec daily -- sudo --user ubuntu --login
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@daily:~$ exit
```

First enable `sudo` for `elementary` ...

```
$ sudo lxc exec daily -- sudo --user root --login
mesg: ttyname failed: No such device
root@daily:~# echo "elementary ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/10-elementary
root@daily:~# exit
```

… and then we rename the user to `elementary`:

```
$ sudo lxc exec daily -- sudo  -s -- <<EOF
usermod -l elementary ubuntu
groupmod -n elementary ubuntu
usermod -d /home/elementary -m elementary
usermod -c "elementary OS Daily" elementary
EOF
```

Now you should be able to jump into the container using the `elementary` user:

```
$ sudo lxc exec daily -- sudo --user elementary --login
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

### Setup elementary OS PPAs

Jump into the newly created `daily` container and setup all needed elementary OS PPAs:

```
elementary@daily:~$ sudo -s -- <<EOF
apt update
apt -y install software-properties-common
add-apt-repository -u -y ppa:elementary-os/os-patches
add-apt-repository -u -y ppa:elementary-os/stable
add-apt-repository -u -y ppa:elementary-os/daily
EOF
```

### Install elementary OS packages

Next install the elementary OS packages in the `daily` container (this may take a while):

```
elementary@daily:~$ sudo -s -- <<EOF
apt update
apt install -y elementary-os-overlay elementary-desktop
apt -y dist-upgrade
apt -y autoremove
apt autoclean
EOF
```

### Configure AppCenter repository in Container

```
elementary@daily:~$ sudo -s -- <<EOF
bash -c 'echo "deb http://packages.elementary.io/appcenter bionic main" >> /etc/apt/sources.list.d/appcenter.list'
wget -O /etc/apt/trusted.gpg.d/appcenter.asc http://packages.elementary.io/key.asc
apt update
EOF
```

### Fix permission issues

```
elementary@daily:~$ sudo -s -- <<EOF
chown elementary:elementary ~/.cache/dconf/user
chown -R elementary:elementary ~/.cache
EOF
```

### Restart LXD Container

Make sure you are in the **Terminal of the host machine** to restart the `daily` container:

```
$ sudo lxc restart daily
```

### Verify

```
$ sudo lxc exec daily -- sudo --user elementary --login
elementary@daily:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	elementary
Description:	elementary OS 5.1.3 Hera
Release:	5.1.3
Codename:	hera

# the following command should start the AppCenter GUI application:
elementary@daily:~$ sudo io.elementary.appcenter
```

### Auto-Update the Container

To install the latest and greatest packages from elementary daily automatically, we setup the following CronJob for `root` in the `daily` container:

```
$ sudo lxc exec daily -- sudo --user elementary --login
elementary@daily:~$ sudo crontab -e
...
#
# m h  dom mon dow   command
0 9 * * * apt update && apt upgrade -y
```

_This means the container gets updated with all the latest packages available every day at 9 AM. Feel free to adjust this however you like ([crontab.guru](https://crontab.guru/) may help creating the right expression)._

To check whether the job was saved sucessfully, execute the following command:

```
elementary@daily:~$ sudo crontab -l
```

### Install the elementary SDK

At this point you setup a completely independent development environment in a LXD container. That said, you can install all needed software within the container without affecting your host machine, which probably runs elementary stable.

In order to be able to develop elementary apps within the container, you need to install the `elementary-sdk` in it:

```
$ sudo lxc exec daily -- sudo --user elementary --login
elementary@daily:~$ sudo apt install elementary-sdk
```

## Usage of daily container

See [this dedicated usage page](Usage-of-Daily-in-LXD-Container.md) for further information how to work with the container.

## Sources

- [linux.marco.betschart.name/elementary-OS/Dell-XPS-7390-Non-Touch-2019.html](https://linux.marco.betschart.name/elementary-OS/Dell-XPS-7390-Non-Touch-2019.html)
- [linux.marco.betschart.name/elementary-OS/Daily.html](https://linux.marco.betschart.name/elementary-OS/Daily.html)
- [blog.simos.info/how-to-easily-run-graphics-accelerated-gui-apps-in-lxd-containers-on-your-ubuntu-desktop/](https://blog.simos.info/how-to-easily-run-graphics-accelerated-gui-apps-in-lxd-containers-on-your-ubuntu-desktop/)

