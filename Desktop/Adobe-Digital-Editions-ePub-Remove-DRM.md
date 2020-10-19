# Adobe Digital Editions ePub Book Remove DRM

![elementary OS: 6 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6%20Odin-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

## Table of Content

- [Setup](#setup)
- [Remove DRM from eBook](#remove-drm-from-ebook)
- [Troubleshooting](#troubleshooting)

## Setup

### Wine 5

In order to be able to run Adobe Digital Editions, you need wine because there is no native Linux client. To do so, make sure you install the latest available stable wine version (> 5), because Adobe Digital Editions runs much better there.

```
sudo apt install wine winetricks winbind
```

### .NET 4.0 Standalone

[Download Microsoft .NET Framework 4 (Standalone Installer)](https://www.microsoft.com/en-us/download/details.aspx?id=17718) and install it using wine:

```
wine ~/Downloads/dotNetFx40_Full_x86_x64.exe
```

### Core Fonts

Next install corefonts using `winetricks`:

```
winetricks corefonts
```

### Adobe Digital Editions

Download Adobe Digital Editions for Windows [from the official Adobe website](https://www.adobe.com/ch_de/solutions/ebook/digital-editions/download.html) and install it with wine:

```
wine ~/Downloads/ADE_4.5_Installer.exe
```

### PyCrypto

Download Python 2.7 and install it in wine:

```
wget -O ~/Downloads/python-2.7.18.amd64.msi https://www.python.org/ftp/python/2.7.18/python-2.7.18.amd64.msi
wine msiexec /i ~/Downloads/python-2.7.18.amd64.msi
```

Download [VCForPython27.msi](https://www.microsoft.com/en-us/download/details.aspx?id=44266) which is needed for PyCrypto. Then install python, pip, vcforpython, and pycyrpt in your wine environment:

```
wine msiexec /i ~/Downloads/VCForPython27.msi
wine "C:\Python27\python.exe" -m pip install pycrypto
wine "C:\Python27\python.exe" -m pip install six
```

### DeDRM Tool

Download the latest release of apprenticeharper's [DeDRM Tool from GitHub](https://github.com/apprenticeharper/DeDRM_tools/releases). Then unzip the files to your wine prefix:

```
cp -p ~/Downloads/DeDRM_tools_6.8.0.zip ~/.wine/drive_c/
(cd ~/.wine/drive_c/ && unzip DeDRM_tools_6.8.0.zip -d DeDRM)
(cd ~/.wine/drive_c/DeDRM && unzip DeDRM_Plugin.zip -d DeDRM_Plugin)
```

### Authorize your Computer

If not done already, launch Adobe Digital Editions and authorize your computer by logging with your Adobe ID. If you don't have one create it for free.

### Obtain `adobekey_1.der` using DeDRM Tools

Also if not done already, make sure you obtain `adobekey_1.der` key file using the DeDRM Tools installed earlier:

```
wine "C:\Python27\python.exe" "C:\DeDRM\DeDRM_Plugin\adobekey.py"
```

## Remove DRM from eBook

Now, all you need to remove DRM from your eBook is its path. By default Adobe Digital Editions stores the protected eBooks in `~/Documents/My Digital Editions/`. To free the eBook from DRM run the following command (**without** Wine):

```
python ~/.wine/drive_c/DeDRM/DeDRM_Plugin/ineptepub.py ~/.wine/drive_c/DeDRM/DeDRM_Plugin/adobekey_1.der ~/Documents/My\ Digital\ Editions/eBook.epub ~/Downloads/eBook-DRM-Free.epub
```

## Further Reading

- [Calibre ePub to PDF conversion settings](Calibre-Settings-PDF-Conversion.md)

## Troubleshooting

### Installation of Adobe Digital Editions crashes

Try to delete your wine prefix and start out with a fresh one:

```
winetricks annihilate
```

### Adobe Digital Editions does not start

Execute the following command in your terminal and analyze the output:

```
wine "C:\Program Files (x86)\Adobe\Adobe Digital Editions 4.5\DigitalEditions.exe"
```

## Sources

- [How to read an ACSM file on Linux? (superuser.com)](https://superuser.com/questions/1027608/how-to-read-an-acsm-file-on-linux)
- [How to install pip with python 2.6 on Windows](https://stackoverflow.com/questions/51560990/how-to-install-pip-with-python-2-6-on-windows)
- [How to install pip for python 2.6?](https://stackoverflow.com/questions/24294467/how-to-install-pip-for-python-2-6)
- [After installing dotnet461 on a win64 prefix, it seems mscoree.dll cannot be found](https://github.com/Winetricks/winetricks/issues/971)
