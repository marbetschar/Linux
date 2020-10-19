# Visual Studio Code: Setup Vala Autocompletion

![Status: WIP](https://img.shields.io/badge/status-wip-ff3130)
![elementary OS: 6 Odin](https://img.shields.io/badge/elementary%C2%A0OS-6%20Odin-007aff)

How to setup Visual Studio Code with Vala autocompletion.

## Prerequisites

- elementary OS 6 Odin
- Installed [VSCodium](https://flathub.org/apps/details/com.vscodium.codium) from Flathub.

## Setup Vala Autocompletion

To begin with, you need to install the [Vala Plugin for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=prince781.vala):

1. Visit the [plugin's website](https://marketplace.visualstudio.com/items?itemName=prince781.vala) and click "Download Extension" (a *.vsix file)
2. Run VSCodium
3. From within VSCodium, open the Extensions tab (`Ctrl + Shift + X`)
4. Click `... > Install from VSIX...` on top of the Extensions sidebar
5. Select the previously downloaded *.vsix file and click `Install`

Now you can install the [Vala Language Server](https://github.com/benwaffle/vala-language-server). For this, open a Terminal and execute the following commands:

```
sudo add-apt-repository ppa:prince781/vala-language-server
sudo apt update
sudo apt install vala-language-server
```

Last but not least, we need to allow flatpak to start the Vala Language Server on the host system. For this, create the new file `~/.bin/vala-language-server`:

```
mkdir -p ~/.bin && vi ~/.bin/vala-language-server
#!/bin/sh
flatpak-spawn --host vala-language-server "$@"
```

Finally, make sure the "Vls: Language Server Path" is set to `/home/$USER/.bin/vala-language-server` in your VSCodium settings. **IMPORTANT:** Make sure to replace the $USER variable with your actual username, because VSCodium does not allow environment variables here.

## Troubleshooting

> `aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template for Elementary/next`

If you encounter this error, get the required information from the the [PPA's website](https://launchpad.net/~prince781/+archive/ubuntu/vala-language-server) and create the apt sources file manually ...

```
sudo vi /etc/apt/sources.list.d/vala-language-server.list
deb http://ppa.launchpad.net/prince781/vala-language-server/ubuntu focal main
deb-src http://ppa.launchpad.net/prince781/vala-language-server/ubuntu focal main
```

... and import the signing key manually as well. For this, you need to copy the portion of the signing key on the [PPA's website](https://launchpad.net/~prince781/+archive/ubuntu/vala-language-server) after the slash - but without the help link at the end:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B13D6EF696260D322CC0980F3C7C9C4B21A1F479
```

Last but not least, update your local package repository cache. Then you should be able to install Vala Language Server:

```
sudo apt update
sudo apt install vala-language-server
```

## Further Reading

- [Vala Plugin for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=prince781.vala)
- [Vala Language Server on GitHub](https://github.com/benwaffle/vala-language-server)
- [How To Add PPA Repository Manually Without "add-apt-repository" On Ubuntu](https://blog.zackad.dev/en/2017/08/17/add-ppa-simple-way.html)
- [Run Vala Language Server from Flatpak](https://github.com/benwaffle/vala-language-server/issues/103#issuecomment-712097579)