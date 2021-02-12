# Visual Studio Code: Setup Vala Autocompletion

![elementary OS: 6 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6%20Odin-007aff)
![Status: Verified](https://img.shields.io/badge/status-verified-58c633)

How to setup VSCodium (Open Source Visual Studio Code) with Vala autocompletion.

## Prerequisites

- elementary OS 6 Odin

## Install VSCodium

Add the GPG key of the repository:

```bash
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/vscodium.gpg
```

Add the repository:

```bash
echo 'deb https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/debs/ vscodium main' | sudo tee --append /etc/apt/sources.list.d/vscodium.list
```

Update package information and install vscodium:

```bash
sudo apt update && sudo apt install codium
```

## Setup Vala Autocompletion

To begin with, you need to install the [Vala Plugin for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=prince781.vala):

1. Visit the [plugin's website](https://marketplace.visualstudio.com/items?itemName=prince781.vala) and click "Download Extension" (a *.vsix file)
2. Run VSCodium
3. From within VSCodium, open the Extensions tab (`Ctrl + Shift + X`)
4. Click `... > Install from VSIX...` on top of the Extensions sidebar
5. Select the previously downloaded *.vsix file and click `Install`

Now you can install the [Vala Language Server](https://github.com/benwaffle/vala-language-server). For this, open a Terminal and execute the following commands:

```bash
sudo add-apt-repository ppa:prince781/vala-language-server
sudo apt install vala-language-server
```

## Further Reading

- [VSCodium: Open Source Binaries of VSCode](https://vscodium.com/)
- [Vala Plugin for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=prince781.vala)
- [Vala Language Server on GitHub](https://github.com/benwaffle/vala-language-server)
- [How To Add PPA Repository Manually Without "add-apt-repository" On Ubuntu](https://blog.zackad.dev/en/2017/08/17/add-ppa-simple-way.html)
