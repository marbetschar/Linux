# Container Cheatsheet

LXD is a next generation system container manager. It offers a user experience similar to virtual machines but using Linux containers instead.

### Management

| Action           | Command                                    |
|:-----------------|:-------------------------------------------|
| List containers  | `lxc list`                                 |
| Create container | `lxc launch <image-name> <container-name>` |
| Delete container | `lxc delete <container-name>`              |

### Daily usage

| Action           | Command                                                                 |
|:-----------------|:------------------------------------------------------------------------|
| Start container  | `lxc snapshot <container-name> <snapshot-name>`                         |
| Stop container   | `lxc restore <container-name> <snapshot-name>`                          |
| Open Terminal    | `lxc exec <container-name> -- sudo --login --user <username>`           |
| Execute command  | `lxc exec <container-name> -- sudo --login --user <username> <command>` |

### Snapshots

| Action              | Command                                         |
|:--------------------|:------------------------------------------------|
| Show container info | `lxc info <container-name>`                     |
| Create snapshot     | `lxc snapshot <container-name> <snapshot-name>` |
| Restore snapshot    | `lxc restore <container-name> <snapshot-name>`  |
| Delete snapshot     | `lxc delete <container-name>/<snapshot-name>`   |

### More

- Images for LXD: [images.linuxcontainers.org](https://images.linuxcontainers.org/)
- LXD REST API: [linuxcontainers.org/lxd/rest-api](https://linuxcontainers.org/lxd/rest-api/)
