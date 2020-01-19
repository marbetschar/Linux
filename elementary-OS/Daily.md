# elementary OS Daily

![elementary OS: 5 Juno](https://img.shields.io/badge/elementary%C2%A0OS-5%20Juno-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install elementary OS Daily - which includes the latest bells and whistles of elementary OS development.

## Prerequisites

- Latest elementary OS installed

## Install elementary OS Daily PPA

You need to add the elementary OS Daily PPA to enable your elementary OS to find the latest available packages.
For this, open a Terminal and insert the following commands:

```
sudo apt install software-properties-common    # installs the "add-apt-repository" command
sudo add-apt-repository ppa:elementary-os/daily
sudo apt update
sudo apt upgrade
```

Et voilà, your on elementary OS Daily now!

For more details see [the PPA's homepage](https://launchpad.net/~elementary-os/+archive/ubuntu/daily).

## Optional: Install elementary OS SDK

If you plan to develop for elementary OS, then you also need to install the elementary OS SDK:

```
sudo apt install elementary-sdk
```