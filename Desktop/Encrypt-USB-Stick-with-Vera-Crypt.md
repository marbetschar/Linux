# Encrypt USB Stick with Vera Crypt

![elementary OS: 6.0 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6.0%20Odin-007aff)
![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)

- [Installation](#installation)
- [Further Reading](#further-reading)
- [Troubleshooting](#troubleshooting)

## Installation

VeraCrypt is not available on the default Ubuntu repositories. Therefore, to install it using the package manger, you have to add the PPA repositories.

Add PPA repos using the command below. Note that this repository is not related to VeraCrypt even though Unit 193 is Xubuntu developer and he is a great contributor to the open source community;

```
sudo add-apt-repository ppa:unit193/encryption
sudo apt-get update
sudo apt install veracrypt
```

## Traveler Disk Setup

**PLEASE NOTE:** Since we are not using Windows, there is no `Tools > Traveler Disk Setup` option. This is expected.
Nevertheless, we can create the Traveler Disk manually:

> ... For linux and MAC this option is not available, but the apps are already portable. I just had to copy the executables
> from `/usr/bin` in Linux and `Applications` in OSX. I've tested them on clean system where TrueCrypt wasn't installed.

### Partition the USB Stick

The first step is to create two Partitions on your USB Stick:

- a small one called `Travel-Disk` using the `NTFS` file system and is `~ 500 MB` in size
    - this partition won't be encrypted: It will provide the VeraCrypt binaries
- a big one claiming all of the remaining space of your USB Stick
    - we are going to encrypt this partition in a minute

### Transfer VeraCrypt binaries to the USB Stick

Next we'll transfer the VeraCrypt binaries to the `Travel-Disk` Partition. Below we are assuming it is mounted under `/media/$USER/Travel-Disk`.

#### Linux

```bash
# Create a Linux directory:
mkdir /media/$USER/Travel-Disk/Linux

# Copy the veracrypt binary:
cp /usr/bin/veracrypt /media/$USER/Travel-Disk/Linux/

# Create a brief README:
cat > /media/$USER/Travel-Disk/Linux/README.md << 'EOF'
# VeraCrypt for Linux

You need to execute the veracrypt binary from your Terminal:

sh -c /media/$USER/Travel-Disk/Linux/veracrypt
EOF
```

#### macOS

Unfortunately for macOS there is no standalone version available, because OSXFUSE 3.10 or newer must be installed too.
So the best we can do is to [download the latest stable VeraCrypt DMG file](https://www.veracrypt.fr/en/Downloads.html) and copy it to `Travel-Disk` together with a brief README file:

```bash
# Create a macOS directory:
mkdir /media/$USER/Travel-Disk/macOS

# Copy the VeraCrypt DMG file:
cp ~/Downloads/VeraCrypt*.dmg /media/$USER/Travel-Disk/macOS/

# Create a brief README explaining the matter:
cat > /media/$USER/Travel-Disk/macOS/README.md << 'EOF'
# VeraCrypt for macOS

- OSXFUSE 3.10 or newer must be installed.

https://www.veracrypt.fr/en/Downloads.html
EOF
```

#### Windows

There is a portable version available for Windows, so we download the [latest stable version](https://www.veracrypt.fr/en/Downloads.html) and copy it to `Travel-Disk/Windows`:

```bash
# Create a Windows directory:
mkdir /media/$USER/Travel-Disk/Windows

# Copy the VeraCrypt Portable exe file:
cp ~/Downloads/VeraCrypt*.exe /media/$USER/Travel-Disk/Windows/
```

### Create encrypted partition

Now everything is ready to create the encrypted partition:

1. Start VeraCrypt
2. Go to `Tools > Volume Creation Wizard`
3. Select `Create a Volume within a partition/drive`
4. Select `Standard VeraCrypt volume`
5. In **Volume Location**, click `Select Device...`
    5.1 then select the previously created, big partition (probably /dev/sda2)
6. Enter the local machine's Administrator password when prompted
7. In **Encryption Options** make sure to select [something different than the defaults](https://blog.elcomsoft.com/2020/03/breaking-veracrypt-containers/)
8. Then set your **Volume Password**
9. `Enable` Large File support
10. Choose `NTFS` as the **Filesystem Type**
11. `Enable` **Cross-Platform Support**
12. Format the Volume

## Further Reading

- [VeraCrypt Documentation](https://www.veracrypt.fr/en/Documentation.html)
- [Create veracrypt traveler disk in Ubuntu](https://askubuntu.com/questions/847127/create-veracrypt-traveler-disk-in-ubuntu)
- [How to work with DMG files on Linux](https://eastmanreference.com/how-to-work-with-dmg-files-on-linux)
- [Breaking VeraCrypt containers](https://blog.elcomsoft.com/2020/03/breaking-veracrypt-containers/)

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