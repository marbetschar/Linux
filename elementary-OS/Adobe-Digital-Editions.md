# Adobe Digital Editions on elementary OS

![elementary OS: 5.1 Hera](https://img.shields.io/badge/elementary%C2%A0OS-5.1%20Hera-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

These steps document how to install Adobe Digital Editions on elementary OS 5.1 Hera.

## Installation

The easiest way to install Adobe Digital Editions is to use `winetricks` - it does all the heavy lifting for you. Please be aware, you also need to install windinb for Adobe Digital Editions to work properly:

```
sudo apt install winbind winetricks
winetricks adobe_diged
```

## Troubleshooting

Make sure you did not tried to install the newer `adobe_diged4`, because this one crashes on elementary OS Hera 5.1.