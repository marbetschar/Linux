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

Remember, if you get an error similar to the following:

> `(io.elementary.code:10031): Gtk-WARNING **: 09:10:55.250: cannot open display: :0`

You'll need to allow usage of display `:0` to the container by executing the following command.
You can automate this by adding it as **Custom Command to System Settings > Applications > Startup**:

```
xhost +local:
```

If you **keep getting the error**, try to restart the `daily` container:

```
lxc restart daily
```

### Enable Vala `debug` messages

Set the environment variable `G_MESSAGES_DEBUG`:

```
lxc exec daily -- sudo --login --user elementary

elementary@daily:~$ vi ~/.profile
export G_MESSAGES_DEBUG=all    # add this line at the end of the file
```

- [Vala, GLib and GTK+ cheatsheet ](https://gist.github.com/matzipan/d4e78f3e5ea95890f403871b50d63b76)

