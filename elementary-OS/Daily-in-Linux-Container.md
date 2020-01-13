# elementary OS daily in Linux Container

![elementary OS: 5 Juno](https://img.shields.io/badge/elementary%C2%A0OS-5%20Juno-007aff)
![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)

## Prerequisites

### LX Daemon

If not done yet, install LXD with the following command:

```
sudo apt install lxd
```

After sucessfull installation, make sure you initialize LXD with:

```
sudo lxd init
Would you like to use LXD clustering? (yes/no) [default=no]: 
Do you want to configure a new storage pool? (yes/no) [default=yes]: yes
Name of the new storage pool [default=default]: containers
Name of the storage backend to use (btrfs, dir, lvm) [default=btrfs]: 
Create a new BTRFS pool? (yes/no) [default=yes]: 
Would you like to use an existing block device? (yes/no) [default=no]: 
Size in GB of the new loop device (1GB minimum) [default=15GB]: 
Would you like to connect to a MAAS server? (yes/no) [default=no]: 
Would you like to create a new local network bridge? (yes/no) [default=yes]: 
What should the new bridge be called? [default=lxdbr0]: 
What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: 
Would you like LXD to be available over the network? (yes/no) [default=no]: 
Would you like stale cached images to be updated automatically? (yes/no) [default=yes] 
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:
```

## Installation

Now you're all set to install elementary OS as container using LXD. For this, ...

## Sources

- https://www.linuxtechi.com/install-lxd-lxc-containers-from-scratch/
