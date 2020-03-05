# macOS Mojave in VMWare Workstation Player

![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)
![macOS: Mojave](https://img.shields.io/badge/macOS-Mojave-blue.svg)
![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![VMWare Workstation Player: 14.1.7](https://img.shields.io/badge/VMWare%C2%A0Workstation%C2%A0Player-14.1.7-007aff)
![unlocker: 3.0.3](https://img.shields.io/badge/unlocker-3.0.3-007aff)

How to setup macOS Mojave in VMWare Player.

## Prerequisites

- macOS Mojave iso image (see [Build macOS Mojave iso if you don't have any yet](#build-macos-Mojave-iso))
- [VMWare Workstation Player v14 installed](https://www.vmware.com/de/products/workstation-player.html)

**PLEASE NOTE:** If your VMWare Player seems to hang on boot and does not even display an operating system splashscreen,
consider starting it as root to see if that fixes the issue.
    
### Build macOS Mojave iso

You only need to do this once. Skip if you already have a macOS Mojave iso image.

**You need access to a Mac to succesfully build a macOS Mojave iso image file.**

Login to your Mac, open the App Store and search for "Mojave". **Download and install "macOS Mojave".**
After successfull installation you are ready to build the iso.

Open the terminal and execute the following commands:

```
#
# PLEASE NOTE: You need at least 18 GB of free disk
# space to sucessfully execute the commands below!

# Create empty Volume which we'll write Mojave to
hdiutil create -o /tmp/macOS-Mojave.cdr -size 8500m -layout SPUD -fs HFS+J

# Mount the created Volume
hdiutil attach /tmp/macOS-Mojave.cdr.dmg -noverify -nobrowse -mountpoint /Volumes/install_build

# let the createinstallmedia executable of Mojave write to the mounted volume
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build

# Unmount the auto mounted Install macOS Mojave Volume
hdiutil detach /Volumes/Install\ macOS\ Mojave

# Convert the *.dmg to iso file
hdiutil convert /tmp/macOS-Mojave.cdr.dmg -format UDTO -o ~/Downloads/macOS-Mojave.iso

# Remove the .cdr suffix of the iso file
mv ~/Downloads/macOS-Mojave.iso.cdr ~/Downloads/macOS-Mojave.iso

# Delete the no longer needed *.dmg
rm /tmp/macOS-Mojave.cdr.dmg
```

## Unlock macOS in VMWare Workstation Player

In order to be able to successfully launch macOS in VMWare Player, we need to unlock the macOS features. To do so install the [paolo-projects/unlocker](https://github.com/paolo-projects/unlocker):

1. Make sure VMWare Workstation Player is closed
2. Download the [latest ZIP Release from GitHub](https://github.com/paolo-projects/unlocker/releases)
3. Unpack the ZIP file
4. Edit the `lnx-install.sh` file and make sure, `gettools.py` is called with `python3` (at the bottom of the file): `python3 gettools.py`
5. Execute `lnx-install.sh` with: `chmod +x lnx-install.sh && sudo ./lnx-install.sh`
6. If the Installation was sucessfull, you are now able to create new virtual machines with macOS as Guest OS.

## Create macOS virtual machine

1. Start the VMWare Workstation Player and select "New Virtual Machine"
2. Select "I will install the operating system later"
3. Select Apple Mac OS X with version "macOS 10.14"
4. Create a virtual disk with **100 GB** capacity and **make sure you enable "Store virtual disk as single file"**
5. Fine tune the virtual machine after creation by click "Edit Virtual Machine":
    - Assign at least 2 CPUs
    - Assign at least 4 GB of RAM
    - **Add a "USB 2.0" controller if you plan to connect an iOS hardware device**
6. **Make sure you close VMWare Workstation Player**

## Install macOS Mojave

1. Start the VMWare Workstation Player **as root:** `sudo vmplayer`
2. Make sure you inserted the macOS Mojave iso image file in the CD/DVD device of the virtual machine (Edit virtual machine settings)
3. Start the virtual machine
4. Once the install media booted, select your language
5. Then, start the **Hard Drive Utility** and **create a new partition**:
    - Select the `VMWare Virtual SATA Hard Drive Media`
    - Hit `Delete` and enter:
        - **Name:** `Macintosh HD`
        - **Format:** `Mac OS Extended (Journaled)`
        - **Schema:** `GUID-Partitionstable`
        - To proceed, hit **Delete** again
    - After your finished, click the red window close icon in the upper left corner
5. Next, start the macOS installation. This should be self explanatory.
    - The installation needs about 30min to complete
