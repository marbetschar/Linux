# Usage of elementary OS Daily in LXD Container

Daily LXD Container usage for elementary OS Desktop development.

## Prerequisites

- [elementary OS Daily installed in an LXD Container named `daily`](Daily-in-LXD-Container.md)

### List available Containers

```
lxc list
```

### Start/Stop Container

```
lxc start daily
```

```
lxc stop daily
```

### Jump into Container

```
lxc exec daily -- sudo --login --user elementary
```

### Start App from Container

```
lxc exec daily -- sudo --login --user elementary io.elementary.code
```
