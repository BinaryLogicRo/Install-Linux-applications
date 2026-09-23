# Install Thunderbird

This setup allows a single user to install and use Thunderbird on their system.

The final result will be a fully functional Thunderbird desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

Note: the default (Debian repository) version of Thunderbird is not used in this setup; Thunderbird is installed manually from the official tarball.

## 1. Download Thunderbird

Download Thunderbird for Linux from: [https://www.thunderbird.net/](https://www.thunderbird.net/).

The downloaded file will be a tarball (e.g., `thunderbird-156.0.tar.xz`).

## 2. Extract the tarball

Using `Ark`, extract the downloaded tarball into the `/home/user/Applications/thunderbird/` directory.

## 3. Run Thunderbird

Navigate to the extracted `thunderbird` folder and double-click the `thunderbird` executable to start the application.

## 4. Create a desktop shortcut

1. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/thunderbird.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=Thunderbird
Comment=Email, chat, and calendaring client
Exec=/home/user/Applications/thunderbird/thunderbird %u
Icon=/home/user/Applications/thunderbird/chrome/icons/default/default256.png
Terminal=false
StartupWMClass=Thunderbird
Type=Application
Categories=Network;Email;Chat;
MimeType=x-scheme-handler/mailto;
Keywords=mail;email;chat;calendar;
SingleMainWindow=true
```

2. Refresh the menu

```bash
kbuildsycoca6
```
