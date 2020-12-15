# Vera Crypt: Encrypt USB Stick

![elementary OS: 6.0 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6.0%20Odin-007aff)
![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)

- [Installation](#installation)
- [Troubleshooting](#troubleshooting)

## Installation

VeraCrypt is not available on the default Ubuntu repositories. Therefore, to install it using the package manger, you have to add the PPA repositories.

Add PPA repos using the command below. Note that this repository is not related to VeraCrypt even though Unit 193 is Xubuntu developer and he is a great contributor to the open source community;

```
sudo add-apt-repository ppa:unit193/encryption
sudo apt-get update
sudo apt install veracrypt
```

## Troubleshooting

### Error: could not find a distribution template for Elementary/odin

> $ sudo add-apt-repository ppa:unit193/encryption    
> Traceback (most recent call last):
>   File "/usr/bin/add-apt-repository", line 108, in <module>
>     sp = SoftwareProperties(options=options)
>   File "/usr/lib/python3/dist-packages/softwareproperties/SoftwareProperties.py", line 118, in __init__
>     self.reload_sourceslist()
>   File "/usr/lib/python3/dist-packages/softwareproperties/SoftwareProperties.py", line 613, in reload_sourceslist
>     self.distro.get_sources(self.sourceslist)    
>   File "/usr/lib/python3/dist-packages/aptsources/distro.py", line 91, in get_sources
>     raise NoDistroTemplateException(
> aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template for Elementary/odin

This error is due to the pre-release nature of elementary 6.0 Odin and should be fixed once the stable version is released.
Meanwhile you can [apply the existing workaround](https://github.com/elementary/os-patches/issues/136#issuecomment-698652540):

_Edit `sudo vi /etc/lsb-release`:_

```diff
-DISTRIB_ID=elementary
-DISTRIB_RELEASE=6
-DISTRIB_CODENAME=odin
-DISTRIB_DESCRIPTION="elementary OS 6 Early Access"
+DISTRIB_ID=Ubuntu
+DISTRIB_RELEASE=20.04
+DISTRIB_CODENAME=focal
+DISTRIB_DESCRIPTION="Ubuntu 20.04.1 LTS"
```

_Edit `sudo vi /etc/os-release`:_

```diff
-NAME="elementary OS"
-VERSION="6 Early Access"
-ID=elementary
-ID_LIKE=ubuntu
-PRETTY_NAME="elementary OS 6 Early Access"
-LOGO=distributor-logo
-VERSION_ID="6"
-HOME_URL="https://elementary.io/"
-DOCUMENTATION_URL="https://elementary.io/docs/learning-the-basics"
-SUPPORT_URL="https://elementary.io/support"
-BUG_REPORT_URL="https://github.com/elementary/os/issues/new"
-PRIVACY_POLICY_URL="https://elementary.io/privacy-policy"
-VERSION_CODENAME=odin
-UBUNTU_CODENAME=focal
+NAME="Ubuntu"
+VERSION="20.04.1 LTS (Focal Fossa)"
+ID=ubuntu
+ID_LIKE=debian
+PRETTY_NAME="Ubuntu 20.04.1 LTS"
+VERSION_ID="20.04"
+HOME_URL="https://www.ubuntu.com/"
+SUPPORT_URL="https://help.ubuntu.com/"
+BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
+PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
+VERSION_CODENAME=focal
+UBUNTU_CODENAME=focal
```

After you changed both files, try to execute `add-apt-repository` again. 