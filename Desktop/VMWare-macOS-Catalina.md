# macOS Catalina in VMWare Workstation Player

![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)
![macOS: Catalina](https://img.shields.io/badge/macOS-Catalina-blue.svg)
![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![VMWare Workstation Player: 15.5.1](https://img.shields.io/badge/VMWare%C2%A0Workstation%C2%A0Player-15.5.1-007aff)
![unlocker: 3.0.3](https://img.shields.io/badge/unlocker-3.0.3-007aff)

How to setup macOS Catalina in VMWare Player.

**PLEASE NOTE:** At the time of writing, the only way to make VMWare Player behave correctly was to start it with root permissions. Otherwise it freezed during startup - showing only a `Waiting for connection...` message.

**IMPORTANT:** The instructions below do not work. For some reason macOS Catalina hangs at boot after initial installation.

## Prerequisites

- macOS Catalina iso image (see [Build macOS Catalina iso if you don't have any yet](#build-macos-catalina-iso))
- [VMWare Workstation Player installed](https://www.vmware.com/de/products/workstation-player.html)

### Build macOS Catalina iso

You only need to do this once. Skip if you already have a macOS Catalina iso image.

**You need access to a Mac to succesfully build a macOS Catalina iso image file.**

Login to your Mac, open the App Store and search for "Catalina". **Download and install "macOS Catalina".**
After successfull installation you are ready to build the iso.

Open the terminal and execute the following commands:

```
#
# PLEASE NOTE: You need at least 18 GB of free disk
# space to sucessfully execute the commands below!

# Create empty Volume which we'll write Catalina to
hdiutil create -o /tmp/macOS-Catalina.cdr -size 8500m -layout SPUD -fs HFS+J

# Mount the created Volume
hdiutil attach /tmp/macOS-Catalina.cdr.dmg -noverify -nobrowse -mountpoint /Volumes/install_build

# let the createinstallmedia executable of Catalina write to the mounted volume
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build

# Unmount the auto mounted Install macOS Catalina Volume
hdiutil detach /Volumes/Install\ macOS\ Catalina

# Convert the *.dmg to iso file
hdiutil convert /tmp/macOS-Catalina.cdr.dmg -format UDTO -o ~/Downloads/macOS-Catalina.iso

# Remove the .cdr suffix of the iso file
mv ~/Downloads/macOS-Catalina.iso.cdr ~/Downloads/macOS-Catalina.iso

# Delete the no longer needed *.dmg
rm /tmp/macOS-Catalina.cdr.dmg
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
3. Select Apple Mac OS X with version "macOS 10.15"
4. Create a virtual disk with **120 GB** capacity and **make sure you enable "Store virtual disk as single file"**
5. Fine tune the virtual machine after creation by click "Edit Virtual Machine":
    - Assign at least 2 CPUs
    - Assign at least 4 GB of RAM
    - **Add a "USB 2.0" controller if you plan to connect an iOS hardware device**
6. **Make sure you close VMWare Workstation Player**

## Install macOS Catalina

1. Start the VMWare Workstation Player **as root:** `sudo vmplayer`
2. Make sure you inserted the macOS Catalina iso image file in the CD/DVD device of the virtual machine (Edit virtual machine settings)
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
