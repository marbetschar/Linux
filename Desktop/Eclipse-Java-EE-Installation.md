# Eclipse Java EE Installation

![elementary OS: 6 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6%20Odin-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-green.svg)

How to install Eclipse Java Enterprise Edition.

## Download

Download the Eclipse IDE for Enterprise Java Developers from the following URL (Linux x86_64):

- [https://www.eclipse.org/downloads/packages/](https://www.eclipse.org/downloads/packages/)

## Extract to `~/Applications/eclipse`

Extract the downloaded *`.tar.gz` file to `~/Applications/eclipse`

## Desktop Integration

As last step, create a Desktop Shortcut for your Launcher. To do so, create a new `~/.local/share/applications/eclipse.desktop` file:

```
[Desktop Entry]
Name=Eclipse Java EE
Comment=Eclipse IDE for Enterprise Java Developers
GenericName=Java IDE
Exec=/home/USER/Applications/eclipse/eclipse
Icon=/home/USER/Applications/eclipse/icon.xpm
Type=Application
StartupNotify=true
Categories=Utility;Java;Development;IDE;
```
