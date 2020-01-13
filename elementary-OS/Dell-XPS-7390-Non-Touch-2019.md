# elementary OS Hera 5.1 on Dell XPS 7390, Non-Touch (13", late 2019)

![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install elementary OS 5.1 Hera on a brand new Dell XPS 7390 Non-Touch system.

**PLEASE NOTE:** We install elementary OS without using the official *.iso image. This strategy **circumvents the following problems**, which are known at the time of this writing (2020-01-11):

- The official *.iso image does not boot [^1]
- After successfull boot from a install media, you'll end up in Dell recovery [^2]
- After successfull install of elementary, several Hardware components do no longer work [^3]

## Factory restore

First of all, make sure you restore the original factory software of the Dell XPS. You can do this using the Dell Recovery Media or Partition.

## Ubuntu configuration

After factory reset you'll end up in the Ubuntu setup configuration. Complete all the steps as you would for using regular Ubuntu. Finally you'll end up in a freshly configured Ubuntu environment.

## Install elementary OS 5.1 Hera

Login to the clean Ubuntu environment, open a Terminal and execute the following commands [^4]:

```
sudo apt-get update
sudo apt-get -y install software-properties-common
sudo add-apt-repository -u -y ppa:elementary-os/os-patches
sudo add-apt-repository -u -y ppa:elementary-os/stable
sudo apt-get install -y elementary-os-overlay elementary-desktop
#
#  ---| Configuring ligthdm |---
#
#             gdm3
#        >>> lightdm <<<
#
#             <Ok>
# ------------------------------
#
sudo apt-get update
sudo apt-get -y dist-upgrade
sudo apt-get -y autoremove
sudo apt-get autoclean
```

## Add elementary Apps repository

We also need to add the repository for AppCenter to install the curated Apps [^5]:

```
sudo bash -c 'echo "deb http://packages.elementary.io/appcenter $(lsb_release -sc) main" >> /etc/apt/sources.list.d/appcenter.list'
sudo wget -O /etc/apt/trusted.gpg.d/appcenter.asc http://packages.elementary.io/key.asc
sudo apt update
```

## Enable the elementary Greeter

Create a new file `/etc/lightdm/lightdm.conf` with the following content [^6]:

```
[Seat:*]
greeter-session=io.elementary.greeter
```

# Reboot

Now we are finished and its time to reboot your system. **Don't be surprised, you'll still see the Ubuntu splash screen** during boot - but you should end up in the elementary Greeter.

# Choose Pantheon Desktop Environment and Login

Once your system sucessfully booted and you'll see the elementary Greeter, make sure you select the Pantheon Desktop Environment by clicking on the Gear Icon. Then Login and voilà, your elementary OS environment is up & running!

## Sources

[^1]: [Dell XPS 13 7390 won't boot from USB](https://www.reddit.com/r/pop_os/comments/dzluza/dell_xps_13_7390_wont_boot_from_usb/f9wf5dv/)

[^2]: [`This Dell Recovery Media can be used to restore the original factory software.`](https://elementaryos.stackexchange.com/questions/21615/how-to-install-on-dell-xps-13-7390-developer-edition)

[^3]: [Install Elementary OS 5.1 Juno on Dell XPS 13 2-in-1 7390](https://alexisanand.com/blog/install-elementary-os-51-juno-on-dell-xps-13-2-in-1-7390/)

[^4]: [Official Elementary OS Juno Docker Image](https://github.com/elementary/docker/blob/master/juno-stable/Dockerfile)

[^5]: [How to install elementary OS AppCenter on Ubuntu](https://askubuntu.com/questions/955227/how-to-install-elementary-os-appcenter-on-ubuntu/955229)

[^6]: [Which configuration files should I change for lightdm and pantheon-greeter](https://askubuntu.com/questions/639733/which-configuration-files-should-i-change-for-lightdm-and-pantheon-greeter)