# ONLYOFFICE Desktop Editors

![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install ONLYOFFICE Desktop Editors on elementary OS 5.1 Hera.

## Add GPG key

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys CB2DE8E5
```

## Add source

```
sudo vi /etc/apt/sources.list.d/onlyoffice.list
deb https://download.onlyoffice.com/repo/debian squeeze main
```

## Install

```
sudo apt update && sudo apt install onlyoffice-desktopeditors
```

## Sources

- [How to install Desktop Editors to Ubuntu and derivatives?](https://helpcenter.onlyoffice.com/desktop/documents/linux/installation-ubuntu.aspx)