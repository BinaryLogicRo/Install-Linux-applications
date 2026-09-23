# Install Balena Etcher

This setup allows a single user to install and use Balena Etcher on their system.

The final result will be a fully functional Balena Etcher desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

## 1. Download Balena Etcher

Download Balena Etcher for Linux from: [https://etcher.balena.io/#download-etcher](https://etcher.balena.io/#download-etcher).

The downloaded file will be a zip archive (e.g., `balenaEtcher-linux-x64-2.1.7.zip`).

## 2. Extract the archive

Using `Ark`, extract the downloaded archive into the `/home/user/Applications/balenaEtcher-linux-x64/` directory.

## 3. Run Balena Etcher

Navigate to the extracted `balenaEtcher-linux-x64` folder and double-click the `balena-etcher` executable to start the application.

## 4. Create a desktop shortcut

1. Download the Balena Etcher icon:

```bash
mkdir -p ~/.local/share/icons
wget -O ~/.local/share/icons/balena-etcher.png https://user-images.githubusercontent.com/5888446/137905045-2ce0cf85-0f7c-4aa5-92ae-9ad5757b8d48.png
```

2. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/balena-etcher.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=Balena Etcher
Comment=Flash OS images to SD cards and USB drives, safely and easily
Exec=/home/user/Applications/balenaEtcher-linux-x64/balena-etcher %U
Icon=/home/user/.local/share/icons/balena-etcher.png
Terminal=false
StartupWMClass=balenaEtcher
Type=Application
Categories=Utility;System;
Keywords=etcher;flash;usb;sd;image;iso;burn;
SingleMainWindow=true
```

3. Refresh the menu

```bash
kbuildsycoca6
```
